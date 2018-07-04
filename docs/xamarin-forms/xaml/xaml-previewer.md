---
title: "XAML Previewer for Xamarin.Forms"
description: "This article explains how to use the XAML Previewer to see your Xamarin.Forms layouts rendered as you type. The XAML Previewer is available in both Visual Studio 2017 and Visual Studio for Mac."
ms.prod: xamarin
ms.assetid: 84769ff1-72fd-4c44-8251-dd6d5bf8c7b2
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/31/2018
---

# XAML Previewer for Xamarin.Forms

_See your Xamarin.Forms layouts rendered as you type!_

## Requirements

Projects require the latest Xamarin.Forms NuGet package for the XAML Previewer to work. Previewing Android apps requires [JDK 1.8 x64](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).

There is more information in the [release notes](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Xamarin_Forms_Previewer).

## Getting Started

# [Visual Studio](#tab/vswin)

XAML Previewer is now on by default and as of 15.8.0 can be controlled from the **Tools > Options > Xamarin > Forms Previewer** in here you can select Default view and Split Orientation.

[![ListView control preview in Visual Studio](xaml-previewer-images/xamlp-options-vs.png "Forms Previewer Options in Visual Studio")](xaml-previewer-images/xamlp-options-vs.png#lightbox "Forms Previewer Options in Visual Studio")

When you open a XAML page to edit the screen will be split according the settings you select in the **Tools > Options** but you can change these during design time using the small controls between the panes.

## XAML Preview Controls

At the top you have the ability to select which pane is in use and this automatcially switches when you click into the pane.  The middle selection switches the panes.

[![ListView control preview in Visual Studio](xaml-previewer-images/xamlp-controls-vs.png "Forms Previewer Pane controls in Visual Studio")](xaml-previewer-images/xamlp-controls-vs.png#lightbox "Forms Previewer Pane controls in Visual Studio")

At the bottom you have the ability to switch the split of the panes between Horizontal and Vertical and also collapse the current sub-pane.

[![ListView control preview in Visual Studio](xaml-previewer-images/xamlp-controls2-vs.png "Forms Previewer Pane controls in Visual Studio")](xaml-previewer-images/xamlp-controls2-vs.png#lightbox "Forms Previewer Pane controls in Visual Studio")

# [Visual Studio for Mac](#tab/vsmac)

The **Preview** button is displayed on the editor when you open a XAML file. The preview pane can be shown or hidden by pressing the **Preview** button in the top-right corner of any XAML document window:

[![ListView control preview in Visual Studio for Mac](xaml-previewer-images/xamlp-list-sml.png "Forms Previewer in Visual Studio for Mac")](xaml-previewer-images/xamlp-list.png#lightbox "Forms Previewer in Visual Studio for Mac")

-----

## XAML Preview Options

The options along the top of the preview pane are:

* **Phone** – render in a phone-size screen
* **Tablet** – render in a tablet-size screen (note there are zoom controls at the bottom-right of the pane)
* **Android** – show the Android version of the screen
* **iOS** – show the iOS version of the screen
* Portait (icon) – uses portrait orientation for the preview
* Landscape (icon) – uses landscape orientation for the preview

## Adding Design-Time Data

Some layouts may be hard to visualize without any data bound to the user interface
controls. To make the preview more useful, assign some static data to the
controls by hardcoding a binding context (either in the code-behind or using XAML).

Refer to James Montemagno's [blog post on adding design-time data](http://motzcod.es/post/143702671962/xamarinforms-xaml-previewer-design-time-data)
to see how to bind to a static ViewModel in XAML.

## Detecting Design Mode

The static [`DesignMode.IsDesignModeEnabled`](xref:Xamarin.Forms.DesignMode.IsDesignModeEnabled) property can be examined to determine whether the application is running in the previewer. This allows you to specify code that will only execute when the application is running in the previewer:

```csharp
if (DesignMode.IsDesignModeEnabled)
{
  // Previewer only code  
}
```

## Troubleshooting

Check the issues below, and the [Xamarin Forums](https://forums.xamarin.com/categories/xamarin-forms),
if you encounter problems.

### XAML Preview isn't showing

Check the following:

* Project should be built (compiled) before attempting to preview XAML files.
* The Designer Agent must be set-up the first time you preview a XAML file - a progress indicator will appear in the Previewer, along with progress messages, until this is ready.
* Try closing and re-opening the XAML file.

### Invalid XAML: The Android project needs to built before preview can be created

The XAML Previewer requires that the project be built before rendering a page.
If the error below appears at the top of the preview pane, re-build the
application and try again.

![Error message: project must be built first](xaml-previewer-images/error-not-built-sml.png "Error message: Rebuild the project")
