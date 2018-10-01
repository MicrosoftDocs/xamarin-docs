---
title: "Wide Color in Xamarin.iOS"
description: "This document discusses wide color and how it can be used in a Xamarin.iOS or Xamarin.Mac app. It also provides a high-level overview of many important color-related concepts such as color spaces, channels, and primaries."
ms.prod: xamarin
ms.assetid: 576E978A-F182-489A-83E4-D8CDC6890B24
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
---

# Wide Color in Xamarin.iOS

_This article covers wide color and how it can be used in a Xamarin.iOS or Xamarin.Mac app._

iOS 10 and macOS Sierra enhances the support for extended-range pixel formats and wide-gamut color spaces throughout the system including frameworks such as Core Graphics, Core Image, Metal and AVFoundation. Support for devices with wide color displays is further eased by providing this behavior throughout the entire graphics stack.

## About Wide Color

As stated above, iOS 10 and macOS Sierra enhances the support for extended-range pixel formats and wide-gamut color spaces throughout the system, including frameworks such as Core Graphics, Core Image, Metal and AVFoundation. Support for devices with wide color displays is further eased by providing this behavior throughout the entire graphics stack.

In the 90's Apple created ColorSync to handle color processing on the Mac. They also helped found the International Color Consortium (ICC) to create and promote a set of standards for handling color on computer hardware. Apple's work with the ICC was included in ColorSync and it was built into the core of OS X (now called macOS).

Apple has also been at the forefront of display technology with hardware such as the Retina Display, the new P3 Display and Display P3 Color Space (released in the iMac in 2015) and the TrueTone displays in the iPad Pros, iPhone 7 and iPhone 7 Plus.

With Wide Color in macOS Sierra and iOS 10, Apple is changing the way that both macOS and iOS handle color to take full advantage of these new display technologies.

## Core Color Concepts

The following core color concepts need to be covered before taking a deeper look at color in macOS and iOS:

### Color Space

A Color Space is an environment in which colors can be represented and compared. It can be a one to four dimensional space that is defined by the intensity of its color components. 

[![](wide-color-images/color00.png "A Color Space")](wide-color-images/color00.png#lightbox)

### Color Channels

The color components can also be referred to as Color Channels. Some familiar representations would be the RGB Spaces, Gray Spaces, CMYK Spaces or Device Independent Spaces. 

[![](wide-color-images/color02.png "The color components can also be referred to as Color Channels")](wide-color-images/color02.png#lightbox)

### Color Primaries

Color Primaries provide the coordinate system that is used to compare and compute colors. Color Primaries usually fall at the most intense version of the given color that can be generated within the Color Channel.

[![](wide-color-images/color01.png "Color Primaries provide the coordinate system that is used to compare and compute colors")](wide-color-images/color01.png#lightbox)

In the case of the RGB Color Space represented above, the Color Primaries are where the `1.0` coordinates are anchored (such as `[1.0, 0.0, 0.0]` for red).

### Color Gamut

Color Gamut refers to all of the colors that can be defined as a combination of the individual Color Channels within a give Color Space.

[![](wide-color-images/color03.png "Color Gamut example")](wide-color-images/color03.png#lightbox)

## What is Wide Color

Before covering the topic of Wide Color, a discussion should be had about the current industry standard for color, the Standard RGB Color Space (sRGB). It Is the most widely used Color Space in computing today and is the default color space for iOS and macOS.

The sRGB Color Space has the following properties:

- It's based on the ITU-R BT.709 standard.
- It has an approximate Gamma of 2.2.
- It represents typical lighting conditions (D65).

Since sRGB is so widely used in the industry, a developer can make some assumptions that the color specified will be faithfully represented on any device it is displayed on. However, this might not always be the case. Additionally, there are several colors that do not fit into the sRGB Color Space and therefor, cannot be represented in it.

For example, many textiles are designed with threads using many inks and dyes falling outside of sRGB. Also many products that a person encounters in their daily life are created with bright, vivid colors that fall outside of the sRGB Color Space. Some of the most compelling examples of colors that cannot be represented in sRGB are things in nature like sunsets, autumn leaves, exotic flowers and tropical waters.

Users who have been capturing digital images in the RAW format may have images on their devices that contain all of this color data, even though it cannot be properly represented in the sRGB Color Space and therefor cannot be properly displayed on screen.

### The Display P3 Color Space

In 2015, Apple released new products (iMac and iPad Pro 9.7") that provide the new Display P3 Color Space to handle the issues created by the sRGB Color Space.

[![](wide-color-images/color04.png "The new Display P3 Color Space")](wide-color-images/color04.png#lightbox)

The Display P3 Color Space has the following properties:

- Supports a wide color space for modern hardware platforms.
- Based on the SMPTE DCI-P3 standard. DCI-P3 was designed for digital projectors but was modified by Apple to support monitors.
- It has an approximate Gamma of 2.2.
- It represents typical lighting conditions (D65).

According to Apple, users are moving their workflows to their mobile platforms. Solving the color presentation issues presented by sRGB in the professional mobile devices (iPad Pros), required more than just including a wide color display. One of the solutions was to upgrade the factory calibration, so each individual device has been calibrated at the factory ensuring that from device to device, color display is accurate and consistent.

Another solution, is the inclusion of full, system-wide color management that Apple has built into iOS 10 and macOS Sierra. 

### The Extended Range sRGB Color Space

The new iOS 10 system-wide color management has to account for all of the existing iOS apps that are built and fine-tuned for sRGB. It was designed to ensure that it didn't impact either color representation or app performance of these existing apps.

To handle this situation, Apple has included the Extended Range sRGB Color Space in iOS 10 (and macOS Sierra as well).

The Extended Range sRGB Color Space has the following properties:

- Has all of the same sRGB Primaries.
- It has an approximate Gamma of 2.2.
- It represents typical lighting conditions (D65).
- It supports negative values and values greater than one (1).

By allowing for values less than zero and greater than one, the Extended Range sRGB Color Space not only allows for existing apps to present colors in sRGB without performance hits or distortion, but it allows the color space to represent any color inside of the visible spectrum. All of this is accomplished while still keeping the same anchor points as the sRGB Color Space.

### Extended Range sRGB in Action

To see how values outside of zero and one work in the Extended Range sRGB Color Space, take the following example of the of the most saturated red available in the Display P3 Color Space:

[![](wide-color-images/color05.png "How values outside of zero and one work in the Extended Range sRGB Color Space")](wide-color-images/color05.png#lightbox)

In Display P3, this color would be represented as `[1.0, 0.0, 0.0]` and in Extended Range sRGB it would be `[1.358, -0.074, -0.012]`. Because sRGB values are full contained inside of Display P3 and the Display P3 values lay "outside" of the sRGB ranges.

For physical hardware that allows pixel values to go from extreme positive to extreme negative values, it can display any color available in the visible spectrum and these values can be represented in the Extended Range sRGB Color Space.

### Device Pixel Formats 

The sRGB Color Space has been largely standardize on using a 8-bit Pixel Format, since 8-bits per color channel is mostly enough to describe colors in sRGB. This is not perfect but good enough, and it gives a good tradeoff between memory and processor usage to display images.

Because Display P3 can represent color coordinates outside of the sRGB color space, it requires 16-bits per color channel to correctly represent colors with the Extended Range sRGB Color Space.

## System-Wide Wide Color Support

To fully support wide color and wide gamut inside of iOS 10 and macOS Sierra, Apple has extended the following frameworks to take full advantage of the Extended Range sRGB Color Space and Display P3:

- UIKit (for iOS only)
- SceneKit
- Core Graphics
- ImageIO
- Core Image
- WebKit
- SpriteKit
- Core Animation
- AppKit (for macOS only)

Additionally, Retina Display support has been enhanced for Extended Range sRGB Color Space and Display P3 displays.

Wide color is supported and can be used in the following application content types:

- Static image resources included in the app bundle.
- Document and network based image resources.
- Advanced Media such as Live Photos or images in the RAW format.
- 3D Graphics shader texture images.

## Solving the Color Problem

The content displayed in an app can come from a wide range of color-rich sources. Additionally, this content can be displayed on a broad range of devices, each with their own range of color display capabilities.

An iOS 10 app bridges the difference between these two issues by using the new built-in, system-wide Color Manage System. This system ensures that an image looks the same on any iOS device, no matter which Color Space the image was encoded in.

Color Management starts with every image having an associated Color Space (or Color Profile). This information is used in the _Color Matching Process_ where colors in the source image are matched to colors in the output device. Since every pixel in the image needs to be Color Matched, it can be time consuming and place a strain upon the device's CPU.

Due to the nature of the _Color Matching Process_, this conversion can be potentially "lossy" if the output device has a smaller gamut than the source image.

Fortunately, the computations that go into the _Color Matching Process_ can easily be hardware accelerated (either on the CPU or GPU) and Apple ensures that it works automatically by building support into base systems such as Quartz 2D, ColorSync and Core Animation. For correctly tagged content, no coding is required to take advantage of these features.

Color Management has been supported on each platform as follows:

- **macOS** - macOS has been color managed since inception.
- **iOS** - iOS has supported automatic color management since iOS 9.3 (and later).

### Designing for Wide Gamut

Apple has the following suggestion for designing and using wide color, wide gamut image content in iOS and macOS apps:

- Only use wide gamut content where in make sense in the app, they should not automatically be used everywhere.
- Only use wide gamut content where vivid colors will enhance the user experience.
- In is not necessary to change all content to P3 for existing apps.

Apple's toolchain makes support for wide gamut image content a gradual opt-in, so supporting wide color in an app is not an all-or-nothing situation.

### Upgrading Existing Content to Wide Color

Apple has the following suggestions for upgrading existing image content to wide color:

- Don't just "assign" a P3 profile to the content in the image editing app. Doing so will simply remap the existing color content to the new Color Space with unexpected results, such as stretching the colors to fit into the new space thus altering the image.
- The image content will need to be "converted" to the Display P3 profile using an image editing app.

### File Formats and Color Profiles

Apple has the following suggestions for the file formats and color profiles used in the app's wide color image contents:

- Use the "Display P3" color profile for RGB working spaces.
- Use a 16-bit per color channel mode.
- Use a Late 2015 iMac (or later) to accurately preview image content.
- Export image assets as 16-bit PNG files with an embedded "Display P3" ICC profile.

> [!IMPORTANT]
> Using the **Save for Web** or **Export Assets** features found in most popular image editing software _will not_ work for wide color images since these features have not been updated to support the required file format specifications yet.

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

## Colors in User Interfaces

When working with the colors in a User Interface, most of the pixels on screen are in a solid color. Additionally, most of these pixels don't come from images assets but are drawn directly by the app (or by the OS on behalf of the app).

Wide gamut color can present several challenges when working at the UI level:

- **Communicating Colors** - When talking about color between designers and developers there is usually an _assumed_ sRGB Color Space involved. So a color might be communicated as `rgb(128, 45, 56)` or `#FF0456`. In a wide gamut design, these representations don't provide enough information to accurately represent the specified color, the working Color Space must also be included. Apple suggests using `P3(128, 45, 56)` and `P3#FF0456` instead. 
- **Picking Colors** - Most of the popular image editing and design software suffer from the same limitations as the sRGB Color Space when using their built-in color pickers. The designer should ensure that they are in the "Display P3" Color Space in the Color Picker when working with wide color designs.
- **Coding Colors** - Both `NSColor` (macOS) and `UIColor` (iOS & tvOS) have new convenience methods for generating P3 colors directly and both have been extended to support colors in the Extended Range sRGB Color Space as well.
- **Storing Colors** - Care should be taken when storing wide gamut colors in an app's document. Where 8-bit per color channel worked fine for the sRGB color space, 16-bit per color channel should be used for wide colors. The developer also needs to watch for instances of assumed Color Space (since everything was traditionally sRGB only).

## Colors on the Web

Care should be taken when working with wide color in web pages and on devices that support wide color display. If all of the image content that has been included in the website has been appropriately tagged, iOS and macOS will automatically color match and display them correctly.

Media queries are also available to help resolve assets between P3 and sRGB capable devices:

```xml
<picture>
	<source srcset="monkey-p3.jpg" media="(color-gamut: p3)">
	<source srcset="monkey-rpg.jpg">
</picture>
```

Apple also has a WebKit proposal that will allow CSS to be specified in other Color Spaces besides the assumed sRGB space.

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

## Summary

This article has covered wide color and the ways that it may be implemented and used inside of a Xamarin.iOS or Xamarin.Mac app.



## Related Links

- [iOS 10 Samples](https://developer.xamarin.com/samples/ios/iOS10/)
