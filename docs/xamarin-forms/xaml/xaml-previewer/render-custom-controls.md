---
title: "Render Custom Controls in the XAML Previewer"
description: "This article describes how to show your custom controls in the XAML Previewer."
ms.prod: xamarin
ms.assetid: 4D795372-CB8F-48F4-B63D-845E44B261F7
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 03/27/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Render Custom Controls in the XAML Previewer

_Custom controls sometimes don't work as expected in the XAML Previewer. Use the guidance in this article to understand the limitations of previewing your custom controls._

## Basic Preview mode

Even if you haven't built your project, the XAML Previewer will render your pages. Until you build, any control that relies on code-behind will show its base Xamarin.Forms type. When your project is built, the XAML Previewer will try to show custom controls with design time rendering enabled. If the render fails, it will show the base Xamarin.Forms type.

## Enable design time rendering for custom controls

If you make your own custom controls, or use controls from a third-party library, the Previewer might display them incorrectly. Custom controls must opt in to design time rendering to appear in the previewer, whether you wrote the control or imported it from a library. With controls you've created, add the [`[DesignTimeVisible(true)]`](xref:System.ComponentModel.DesignTimeVisibleAttribute) to your control's class to show it in the Previewer:

```csharp
namespace MyProject
{
  [DesignTimeVisible(true)]
  public class MyControl : BaseControl
  {
    // Your control's code here
  }

}
```

Use [James Montemagno's ImageCirclePlugin's base class](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs) as an example.

## SkiaSharp controls

Currently, SkiaSharp controls are only supported when you're previewing on iOS. They won't render on the Android preview.

## Troubleshooting

### Check your Xamarin.Forms version
Make sure you have at least Xamarin.Forms 3.6 installed. You can update your Xamarin.Forms version on NuGet.

### Even with `[DesignTimeVisible(true)]`, my custom control isn't rendering properly.
Custom controls that rely heavily on code-behind or backend data don't always work in the XAML Previewer. You can try:

* Moving the control so it doesn't initialize if [design mode is enabled](index.md#detect-design-mode)
* Setting up [design time data](design-time-data.md) to show fake data from the backend

### The XAML Previewer shows the error "Custom Controls aren't rendering properly"
Try cleaning and rebuilding your project, or closing and reopening the XAML file.
