---
title: "Xamarin.Forms Material Visual"
description: "This article introduces Xamarin.Forms Material Visual, which renders views identically, or largely identically, using Material Design guidelines on iOS and Android."
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2018
---

# Xamarin.Forms Material Visual

_This article introduces Xamarin.Forms Material Visual, which renders views identically, or largely identically, on iOS and Android._

Many developers want to create Xamarin.Forms applications that look identical, or largely identical, on iOS and Android. Xamarin.Forms 3.6 includes a mechanism for including additional renderers that implement a material appearance.

```xaml
<ContentPage ...
             Visual="Material">
    ...
</ContentPage>
```

```c#
var contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

Xamarin.Forms 3.6 includes a visual appearance based on material design. Material renderers are currently included for the following views on iOS and Android:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)

Functionally, the material renderers are no different to the default renderers.

On iOS and Android your platform project must have the [https://www.nuget.org/packages/Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet package installed and add the following after `Forms.Init`

## iOS

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

## Android

```csharp
global::Xamarin.Forms.Forms.Init(this, bundle);
FormsMaterial.Init(this, bundle);
```

 On Android, material only  works with TargetFramework 9.0 (API 28), your platform project must use v28 of the support libraries, and its theme needs to inherit from a Material Components theme or continue to inherit from an AppCompat theme. For more information, see [Getting started with Material Components for Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

The following screenshots show a user interface that includes the four views for which material renderers exist, but rendered using the default renderers:

[![Default renderers](visual-images/default-renderers.png "Views using default renderers")](visual-images/default-renderers-large.png#lightbox)

The following screenshots show the same user interface rendered using the material renderers:

[![Material renderers](visual-images/material-renderers.png "Views using material renderers")](visual-images/material-renderers-large.png#lightbox)

> [!NOTE]
> With material renderers the rendered controls remain native controls, and therefore there will still be user interface differences between platforms for areas such as fonts, shadows, colors, and elevation.

### Material Renderers

Material Renderers can be customized just like default renderers using the following base classes

- MaterialButtonRenderer
- MaterialEntryRenderer
- MaterialFrameRenderer
- MaterialProgressBarRenderer
- MaterialDatePickerRenderer
- MaterialTimePickerRenderer
- MaterialPickerRenderer
- MaterialActivityIndicatorRenderer
- MaterialEditorRenderer
- MaterialSliderRenderer
- MaterialStepperRenderer

#### Example
```C#
using Xamarin.Forms.Material.Android

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer

```

## Related Links

- [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
