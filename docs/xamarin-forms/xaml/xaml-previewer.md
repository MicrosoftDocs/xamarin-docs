---
title: "XAML Previewer for Xamarin.Forms"
description: "This article explains how to use the XAML Previewer to see your Xamarin.Forms layouts rendered as you type. The XAML Previewer is available in Visual Studio 2017, Visual Studio for Mac, and Visual Studio 2019 (Preview)."
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 02/04/2019
---

# XAML Previewer for Xamarin.Forms

_See your Xamarin.Forms layouts rendered as you type!_

## Requirements

Projects require the latest Xamarin.Forms NuGet package for the XAML Previewer to work. Previewing Android apps requires [JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

There is more information in the [release notes](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer).

## Getting started

The Xamarin.Forms XAML Previewer shows a platform-specific preview of your XAML file in real time, while you edit it.

::: zone pivot="windows"

### Visual Studio

The XAML Previewer is available by expanding the split view pane. Default split view behavior can be controlled from the **Tools > Options > Xamarin > Forms Previewer** dialog. In this dialog, you can select the default document view and the split orientation.

[![Xamarin.Forms Previewer options in Visual Studio](xaml-previewer-images/xamlp-options-vs-sm.png "Xamarin.Forms Previewer options in Visual Studio")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

When opening a XAML page, the editor will split based on the settings selected in the **Tools > Options > Xamarin > Forms Previewer** dialog. However, the split can be changed for each file in the editor window.

#### XAML preview controls

The top of the editor window has buttons to select which pane is in use, with the top button switching to the design pane and the bottom button switching to the source pane. The middle button swaps the pane order.

[![Xamarin.Forms Previewer controls to switch between design, source, and split view in Visual Studio](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin.Forms Previewer controls to switch between design, source, and split view in Visual Studio")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

The bottom of the editor window has buttons to vertically and horizontally split the panes, and to expand or collapse the current sub-pane.

[![Xamarin.Forms Previewer pane orientation controls in Visual Studio](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Xamarin.Forms Previewer pane orientation controls in Visual Studio")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end
::: zone pivot="macos"

### Visual Studio for Mac

The **Preview** button is displayed on the editor when you open a XAML page. The preview pane can be shown or hidden by pressing the **Preview** button in the top-right corner of any XAML document window:

[![Xamarin.Forms Previewer in Visual Studio for Mac](xaml-previewer-images/xamlp-list-sml.png "Xamarin.Forms Previewer in Visual Studio for Mac")](xaml-previewer-images/xamlp-list.png#lightbox)

::: zone-end
::: zone pivot="win-dev16"

### Visual Studio 2019 (Preview)

The XAML Previewer is available by expanding the split view pane. Default split view behavior can be controlled from the **Tools > Options > Xamarin > Forms Previewer** dialog. In this dialog, you can select the default document view and the split orientation.

[![Xamarin.Forms Previewer options in Visual Studio](xaml-previewer-images/xamlp-options-vs-sm.png "Xamarin.Forms Previewer options in Visual Studio")](xaml-previewer-images/xamlp-options-vs-lg.png#lightbox)

When opening a XAML page, the editor will split based on the settings selected in the **Tools > Options > Xamarin > Forms Previewer** dialog. However, the split can be changed for each file in the editor window.

#### XAML preview controls

The top of the editor window has buttons to select which pane is in use, with the top button switching to the design pane and the bottom button switching to the source pane. The middle button swaps the pane order.

[![Xamarin.Forms Previewer controls to switch between design, source, and split view in Visual Studio](xaml-previewer-images/xamlp-controls-splitview-vs-sm.png "Xamarin.Forms Previewer controls to switch between design, source, and split view in Visual Studio")](xaml-previewer-images/xamlp-controls-splitview-vs-lg.png#lightbox)

The bottom of the editor window has buttons to vertically and horizontally split the panes, and to expand or collapse the current sub-pane.

[![Xamarin.Forms Previewer pane orientation controls in Visual Studio](xaml-previewer-images/xamlp-controls-orientation-vs-sm.png "Xamarin.Forms Previewer pane orientation controls in Visual Studio")](xaml-previewer-images/xamlp-controls-orientation-vs-lg.png#lightbox)

::: zone-end

## XAML previewer options

The options along the top of the preview pane are:

* **Phone** – preview on a phone sized screen
* **Tablet** - preview on a tablet sized screen
* **Android** – show the Android version of the screen
* **iOS** – show the iOS version of the screen (*Note: If you're using Visual Studio on Windows, you must be [paired to a Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) to use this mode*)
* **Portrait (icon)** – uses portrait orientation for the preview
* **Landscape (icon)** – uses landscape orientation for the preview

::: zone pivot="win-dev16"

## Device drop-down menu

In Visual Studio 2019 (Preview), you can select a device to preview on from a drop-down list of Android or iOS devices. This drop-down replaces the "Phone" and "Tablet" options. When previewing iOS, the drop-down will show iPhone and iPad devices. When previewing Android, the drop-down will show various Android phones and tablets. Both lists include screen sizes and screen resolutions.

::: zone-end

## Detect design mode

The static [`DesignMode.IsDesignModeEnabled`](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) property can be examined to determine whether the application is running in the previewer. This allows you to specify code that will only execute when the application is running in the previewer:

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

::: zone pivot="win-dev16"

## Enable design time rendering for custom controls

Custom controls have to opt-in to design time rendering to appear in the previewer, regardless of whether the control resides in your project, or is imported from a library. This can be accomplished by adding the [`[DesignTimeVisible(true)]`](xref:System.ComponentModel.DesignTimeVisibleAttribute) to your control's class:

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

An example of this can be seen in [James Montemagno's ImageCirclePlugin's base class](https://github.com/jamesmontemagno/ImageCirclePlugin/blob/master/src/ImageCircle/CircleImage.shared.cs).

::: zone-end

## Add design-time data

Some layouts may be hard to visualize without any data bound to the user interface
controls. To make the preview more useful, assign some static data to the
controls by hardcoding a binding context (either in the code-behind or using XAML).

Refer to James Montemagno's [blog post on adding design-time data](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data) to see how to bind to a static ViewModel in XAML.

## Troubleshooting

Check the issues below, and the [Xamarin Forums](https://forums.xamarin.com/categories/xamarin-forms),
if you encounter problems.

### XAML Previewer isn't showing or shows an error

Check the following:

* The Designer Agent must be set-up the first time you preview a XAML file - a progress indicator will appear in the Previewer, along with progress messages, until this is ready.
* Try closing and re-opening the XAML file.
* Ensure that your `App` class has a parameterless constructor.

::: zone pivot="win-dev16"

### Custom controls aren't rendering

Ensure that your project is built. If the previewer can't render the control without an error, or the control's creator did not opt-in to design time rendering, the previewer shows the control's base class.

::: zone-end
