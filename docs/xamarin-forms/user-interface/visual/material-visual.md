---
title: "Xamarin.Forms Material Visual"
description: "Xamarin.Forms Material Visual can be used to create Xamarin.Forms applications that look identical, or largely identical, on iOS and Android."
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
---

# Xamarin.Forms Material Visual

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)

[Material Design](https://material.io) is an opinionated design system created by Google, that prescribes the size, color, spacing, and other aspects of how views and layouts should look and behave.

Xamarin.Forms Material Visual can be used to apply Material Design rules to Xamarin.Forms applications, creating applications that look identical, or largely identical, on iOS and Android. When Material Visual is enabled, supported views adopt the same design cross-platform, creating a unified look and feel. This is achieved with material renderers, that apply the Material Design rules.

The process for enabling Xamarin.Forms Material Visual in your application is:

1. Add the [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet package to your iOS and Android platform projects. This NuGet package delivers optimized Material Design renderers on iOS and Android. On iOS, the package provides the transitive dependency to [Xamarin.iOS.MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents), which is a C# binding to Google's [Material Components for iOS](https://material.io/develop/ios/). On Android, the package provides build targets to ensure that your TargetFramework is correctly set up.
1. Initialize the material renderers in each platform project. For more information, see [Initialize material renderers](#initialize-material-renderers).
1. Consume the material renderers by setting the [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) property to `Material` on any pages that should adopt the Material Design rules. For more information, see [Consume material renderers](#consume-material-renderers).
1. [optional] Customize the material renderers. For more information, see [Customize material renderers](#customize-material-renderers).

> [!IMPORTANT]
> On Android, the material renderers require a minimum version of 5.0 (API 21) or greater, and a TargetFramework of version 9.0 (API 28). In addition, your platform project requires Android support libraries 28.0.0 or greater, and its theme needs to inherit from a Material Components theme or continue to inherit from an AppCompat theme. For more information, see [Getting started with Material Components for Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Material renderers are currently included in the [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet package for the following views:

- [`Button`](xref:Xamarin.Forms.Button)
- `CheckBox`
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

## Initialize material renderers

After installing the [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet package, the material renderers must be initialized in each platform project.

On iOS, this should occur in **AppDelegate.cs** by invoking the `FormsMaterial.Init` method *after* the `Xamarin.Forms.Forms.Init` method:

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

On Android, this should occur in **MainActivity.cs** by invoking the `FormsMaterial.Init` method *after* the `Xamarin.Forms.Forms.Init` method:

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
FormsMaterial.Init(this, savedInstanceState);
```

## Consume material renderers

Applications can opt into using the material renderers by setting the [`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual) property on a page, layout, or view, to `Material`:

```xaml
<ContentPage Visual="Material"
             ...>
    ...
</ContentPage>
```

The equivalent C# code is:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

The [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) property can be set to any type that implements `IVisual`, with the [`VisualMarker`](xref:Xamarin.Forms.VisualMarker) class providing the following `IVisual` properties:

- `Default` – indicates that the view should render using the default renderer.
- `MatchParent` – indicates that the view should use the same renderer as its direct parent.
- `Material` – indicates that the view should render using a material renderer.

> [!IMPORTANT]
> The [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) property is defined in the [`VisualElement`](xref:Xamarin.Forms.VisualElement) class, with views inheriting the `Visual` property value from their parents. Therefore, setting the `Visual` property on a [`ContentPage`](xref:Xamarin.Forms.ContentPage) ensures that any supported views in the page will use that Visual. In addition, the `Visual` property can be overridden on a view.

The following screenshots show a user interface rendered using the default renderers:

[![Screenshot of default renderers, on iOS and Android](material-visual-images/default-renderers.png "Views using default renderers")](material-visual-images/default-renderers-large.png#lightbox)

The following screenshots show the same user interface rendered using the material renderers:

[![Screenshot of material renderers, on iOS and Android](material-visual-images/material-renderers.png "Views using material renderers")](material-visual-images/material-renderers-large.png#lightbox)

The main visible differences between the default renderers and material renderers, shown here, are that the material renderers capitalize [`Button`](xref:Xamarin.Forms.Button) text, and round the corners of [`Frame`](xref:Xamarin.Forms.Frame) borders. However, material renderers use native controls, and therefore there may still be user interface differences between platforms for areas such as fonts, shadows, colors, and elevation.

> [!NOTE]
> Material Design components adhere closely to Google's guidelines. As a result, Material Design renderers are biased towards that sizing and behavior. When you require greater control of styles or behavior, you can still create your own [Effect](~/xamarin-forms/app-fundamentals/effects/index.md), [Behavior](~/xamarin-forms/app-fundamentals/behaviors/index.md), or [Custom Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) to achieve the detail you require.

## Customize material renderers

Material renderers can optionally be customized, just like the default renderers, through the following base classes:

- `MaterialButtonRenderer`
- `MaterialCheckBoxRenderer`
- `MaterialEntryRenderer`
- `MaterialFrameRenderer`
- `MaterialProgressBarRenderer`
- `MaterialDatePickerRenderer`
- `MaterialTimePickerRenderer`
- `MaterialPickerRenderer`
- `MaterialActivityIndicatorRenderer`
- `MaterialEditorRenderer`
- `MaterialSliderRenderer`
- `MaterialStepperRenderer`

The following code shows an example of customizing the `MaterialProgressBarRenderer` class:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer
    {
        ...
    }
}
```

In this example, the `ExportRendererAttribute` specifies that the `CustomMaterialProgressBarRenderer` class will be used to render the [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) view, with the `IVisual` type registered as the third argument.

> [!NOTE]
> A renderer that specifies an `IVisual` type, as part of its `ExportRendererAttribute`, will be used to render opted in views, rather than the default renderer. At renderer selection time, the `Visual` property of the view is inspected and included in the renderer selection process.

For more information about custom renderers, see [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## Related links

- [Material Visual (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)
- [Create a Xamarin.Forms Visual Renderer](create.md)
- [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
