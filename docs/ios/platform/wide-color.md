---
title: "Wide Color in Xamarin.iOS"
description: "This document discusses wide color and how it can be used in a Xamarin.iOS or Xamarin.Mac app."
ms.service: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
no-loc: [Objective-C]
---

# Wide Color in Xamarin.iOS

_This article covers wide color and how it can be used in a Xamarin.iOS or Xamarin.Mac app._

iOS 10 and macOS Sierra enhances the support for extended-range pixel formats and wide-gamut color spaces throughout the system including frameworks such as Core Graphics, Core Image, Metal, and AVFoundation. Support for devices with wide color displays is further eased by providing this behavior throughout the entire graphics stack.

## Asset Catalogs

### Supporting Wide Color with Asset Catalogs

In iOS 10 and macOS Sierra, Apple has expanded the Asset Catalogs used to include and categorize static image content in the app's bundle to support wide color.

Using Asset Catalogs provide the following benefits to an app:

- They provide the best deployment option for static image assets.
- They support automatic color correction.
- They support automatic pixel format optimization.
- They support App Slicing and App Thinning which ensures that only the content that is relevant get's delivered to the end user's device.

Apple has made the following enhancements to Asset Catalogs for wide color support:

- They support 16-bit (per color channel) source content.
- They support cataloging content by display gamut. Content can be tagged for either the sRGB or Display P3 gamuts.

The developer has three options for supporting wide color content in their apps:

1. **Do Nothing** - Since wide color content should only be used in situations where the app needs to display vivid colors (where they will enhance the user's experience), content outside of this requirement should be left as-is. It will continue to be rendered correctly in all hardware situations.
2. **Upgrade Existing Content to Display P3** - This requires the developer to replace the existing image content in their Asset Catalog with a new, upgraded file in the P3 format and tag it as such in the Asset Editor. At build time, a sRGB derivative image will be generated from these assets.
3. **Provide Optimized Asset Content** - In this situation, the developer will provide both an 8-bit sRGB and a 16-bit Display P3 vision of each image in the Asset Catalog (properly tagged in the asset editor).

### Asset Catalog Deployment

The following will occur when the developer submits an app to the App Store with Asset Catalogs that include wide color image content:

- When the app is deployed to the end user, App Slicing will ensure that only the appropriate content variant is delivered to the user's device.
- On device that don't support wide color, there is no payload cost for including wide color content, as it is never shipped to the device.
- `NSImage` on macOS Sierra (and later) will automatically select the best content representation for the hardware's display.
- The displayed content will be refreshed automatically when or if the devices hardware display characteristics change.

### Asset Catalog Storage

Asset Catalog storage has the following properties and implications for an app:

- At build time, Apple attempts to optimize the storage of the image content via high quality image conversions.
- 16-bits are used per color channel for wide color content.
- Content dependent image compression is used to lower deliverable content sizes. New "lossy" compressions have been added to further optimize content sizes.

## Rendering Off-Screen Images in App

Based on the type of app being created, it might allow the user to include image content they have collected from the internet or create image content directly inside of the app (like a vector drawing app for example).

In both of these cases, the app can render the required imagery off-screen in wide color using enhanced features added to both iOS and macOS.

### Drawing Wide Color in iOS

Before discussing how to correctly draw a wide color image in iOS 10, take a look at the following common iOS drawing code:

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    UIGraphics.BeginImageContext (size);

    ...

    UIGraphics.EndImageContext ();
    return image = UIGraphics.GetImageFromCurrentImageContext ();
    }
```

There are issues with the standard code that will need to be addressed _before_ it can be used to draw a wide color image. The `UIGraphics.BeginImageContext (size)` method used to start iOS image drawing has the following limitations:

- It cannot create image contexts with more than 8 bits per color channel.
- It cannot represent colors in the Extended Range sRGB Color Space.
- It does not have the ability to provide an interface to create contexts in a non-sRGB Color Space because of the low-level C routines being called in the background.

To handle these limitations and draw a wide color image in iOS 10, use the following code instead:

```csharp
public UIImage DrawWideColorImage ()
{
    var size = new CGSize (250, 250);
    var render = new UIGraphicsImageRenderer (size);

    var image = render.CreateImage ((UIGraphicsImageRendererContext context) => {
        var bounds = context.Format.Bounds;
        CGRect slice;
        CGRect remainder;
        bounds.Divide (bounds.Width / 2, CGRectEdge.MinXEdge, out slice, out remainder);

        var redP3 = UIColor.FromDisplayP3 (1.0F, 0.0F, 0.0F, 1.0F);
        redP3.SetColor ();
        context.FillRect (slice);

        var redRGB = UIColor.Red;
        redRGB.SetColor ();
        context.FillRect (remainder);
    });

    // Return results
    return image;
}
```

The new `UIGraphicsImageRenderer` class creates a new image context that is capable of handling the Extended Range sRGB Color Space and it has the following features:

- It is fully color managed by default.
- It supports the Extended Range sRGB Color Space by default.
- It intelligently decides if it should render in the sRGB or Extended Range sRGB Color Space based on the capabilities of the iOS device that the app is running on.
- It fully and automatically manages the image context (`CGContext`) lifetime so the developer doesn't have to worry about calling begin and end context commands.
- It is compatible with the `UIGraphics.GetCurrentContext()` method.

The `CreateImage` method of the `UIGraphicsImageRenderer` class is called to create a wide color image and passed a completion handler with the image context to draw into. All of the drawing is done inside of this completion handler as follows:

- The `UIColor.FromDisplayP3` method creates a new fully saturated red color in the wide gamut Display P3 Color Space and it is used to draw the first half of the rectangle. 
- The second half of the rectangle is drawn in the normal sRGB fully saturated red color for comparison.

### Drawing Wide Color in macOS

The `NSImage` class has been expanded in macOS Sierra to support the drawing of Wide Color images. For example:

```csharp
var size = CGSize(250,250);
var wideColorImage = new NSImage(size, false, (drawRect) =>{
    var rects = drawRect.Divide(drawRect.Size.Width/2,CGRect.MinXEdge);

    var color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    var path = new NSBezierPath(rects.Slice).Fill();

    color = new NSColor(1.0, 0.0, 0.0, 1.0).Set();
    path = new NSBezierPath(rects.Remainder).Fill();

    // Return modified
    return true;
});
```

## Rendering On-Screen Images in App

To render wide color images on-screen, the process works similar to drawing an off-screen wide color image for macOS and iOS presented above.

### Rendering On-Screen in iOS

When the app needs to render an image in wide color on-screen in iOS, override the `Draw` method of the `UIView` in question as usual. For example:

```csharp
using System;
using UIKit;
using CoreGraphics;

namespace MonkeyTalk
{
    public class MonkeyView : UIView
    {
        public MonkeyView ()
        {
        }

        public override void Draw (CGRect rect)
        {
            base.Draw (rect);

            // Draw the image to screen
        ...
        }
    }
}
```

As iOS 10 does with the `UIGraphicsImageRenderer` class shown above, it intelligently decides if it should render in the sRGB or Extended Range sRGB Color Space based on the capabilities of the iOS device that the app is running on when the `Draw` method is called. Additionally, the `UIImageView` has been color managed since iOS 9.3 as well.

If the app needs to know how rendering is being done on a `UIView` or `UIViewController`, it can check the new `DisplayGamut` property of the `UITraitCollection` class. This value will be a `UIDisplayGamut` enum of the following:

- P3
- Srgb
- Unspecified

If the app wants to control which Color Space is used to draw an image, it can use a new `ContentsFormat` property of the `CALayer` to specify the desired Color Space. This value can be a `CAContentsFormat` enum of the following:

- Gray8Uint
- Rgba16Float
- Rgba8Uint

### Rendering On-Screen in macOS

When the app needs to render an image in wide color on-screen in macOS, override the `DrawRect` method of the `NSView` in question as usual. For example:

```csharp
using System;
using AppKit;
using CoreGraphics;
using CoreAnimation;

namespace MonkeyTalkMac
{
    public class MonkeyView : NSView
    {
        public MonkeyView ()
        {
        }

        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Draw graphics into the view
            ...
        }
    }
}
```

Again, it intelligently decides if it should render in the sRGB or Extended Range sRGB Color Space based on the capabilities of the Mac hardware that the app is running on when the `DrawRect` method is called.

If the app wants to control which Color Space is used to draw an image, it can use a new `DepthLimit` property of the `NSWindow` class to specify the desired Color Space. This value can be a `NSWindowDepth` enum of the following:

- OneHundredTwentyEightBitRgb
- SixtyfourBitRgb
- TwentyfourBitRgb
