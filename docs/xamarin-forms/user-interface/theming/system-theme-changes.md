---
title: "Respond to system theme changes in Xamarin.Forms applications"
description: "Xamarin.Forms applications can respond to operating system theme changes by using the OnAppTheme type, and the DynamicResource markup extension."
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.prod: xamarin
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
---

# Respond to system theme changes in Xamarin.Forms applications

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)

Devices typically include light and dark themes, which each refer to a broad set of appearance preferences that can be set at the operating system level. Applications should respect these system themes, and respond immediately when the system theme changes.

The system theme may change for a variety of reasons, depending on the device configuration. This includes the system theme being explicitly changed by the user, it changing due to the time of day, and it changing due to environmental factors such as low light.

Xamarin.Forms applications can respond to system theme changes by defining resources with the `AppThemeColor` class, the `OnAppTheme<T>` class, and the `OnAppTheme` markup extension. These resources should then be consumed with the `DynamicResource` markup extension.

> [!IMPORTANT]
> Responding to a system theme change is currently experimental, and can only be used by setting the `AppTheme_Experimental` flag. For more information, see [Experimental Flags](~/xamarin-forms/internals/experimental-flags.md).

The following requirements must be met for Xamarin.Forms to respond to a system theme change:

- Xamarin.Forms 4.6 or greater.
- iOS 13 or greater.
- Android 10 (API 29) or greater.
- UWP build 14393 or greater.

The following screenshots show themed pages, for light and dark system themes on iOS and Android:

[![Screenshot of the main page of a themed app, on iOS and Android](system-theme-changes-images/main-page-both-themes.png "Main page of themed app")](system-theme-changes-images/main-page-both-themes-large.png#lightbox "Main page of themed app")
[![Screenshot of the detail page of a themed app, on iOS and Android](system-theme-changes-images/detail-page-both-themes.png "Detail page of themed app")](system-theme-changes-images/detail-page-both-themes-large.png#lightbox "Detail page of themed app")

## Define and consume theme resources

Resources for light and dark themes can be defined with the `AppThemeColor` class, the `OnAppTheme<T>` class, and the `OnAppTheme` markup extension. With each approach, these resources are automatically applied based on the value of the current system theme. In addition, objects that consume these resources are automatically updated if the system theme changes while an app is running.

### AppThemeColor

The `AppThemeColor` class is used to define [`Color`](xref:Xamarin.Forms.Color) resources for the light and dark system themes. `AppThemeColor` resources should be defined in a [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Application ...>
    <Application.Resources>
        <AppThemeColor x:Key="PageBackgroundColor"
                       Light="White"
                       Dark="Black" />
        <AppThemeColor x:Key="NavigationBarColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="PrimaryColor"
                       Light="WhiteSmoke"
                       Dark="Teal" />
        <AppThemeColor x:Key="SecondaryColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="PrimaryTextColor"
                       Light="Black"
                       Dark="White" />
        <AppThemeColor x:Key="SecondaryTextColor"
                       Light="White"
                       Dark="White" />
        <AppThemeColor x:Key="TertiaryTextColor"
                       Light="Gray"
                       Dark="WhiteSmoke" />
        <AppThemeColor x:Key="TransparentColor"
                       Light="Transparent"
                       Dark="Transparent" />
    </Application.Resources>
</Application>
```

Each `AppThemeColor` resource must have a `x:Key` attribute, which gives it a descriptive key in the [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). The value of the `Light` and `Dark` properties should be [`Color`](xref:Xamarin.Forms.Color) objects. In addition, a `Default` property can be set to a `Color` to be used by the consuming object by default.

`AppThemeColor` resources can be consumed inline:

```xaml
<Label Text="This monkey reacts appropriately to ridiculous assertions and actions"
       TextColor="{DynamicResource PrimaryTextColor}" />
```

Alternatively, `AppThemeColor` resources can be consumed by implicit or explicit [`Style`](xref:Xamarin.Forms.Style) objects:

```xaml
<Style TargetType="NavigationPage">
    <Setter Property="BarBackgroundColor"
            Value="{DynamicResource NavigationBarColor}" />
    <Setter Property="BarTextColor"
            Value="{DynamicResource SecondaryColor}" />
</Style>
```

> [!IMPORTANT]
> `AppThemeColor` resources should be consumed with the `DynamicResource` markup extension. This ensures that the consuming object's appearance is updated when the system theme changes.

### OnAppTheme&lt;T&gt;

The `OnAppTheme<T>` class is used to define resources of any type for the light and dark system themes. `OnAppTheme<T>` resources should be defined in a [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), with the `T` argument specified as the value of the `x:TypeArguments` attribute:

```xaml
<Application ...>
    <Application.Resources>
        <OnAppTheme x:Key="ImageLogo"
                    x:TypeArguments="FileImageSource"
                    Light="lightlogo.png"
                    Dark="darklogo.png" />
    </Application.Resources>
</Application>
```

Each `OnAppTheme<T>` resource must have a `x:Key` attribute, which gives it a descriptive key in the [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary). The value of the `Light` and `Dark` properties should be objects of the type defined as the `x:TypeArguments` attribute. In addition, a `Default` property can be set to an object of type `T` to be used by the consuming object by default.

`OnAppTheme<T>` resources can be consumed inline:

```xaml
<Image Source="{DynamicResource ImageLogo}"
       Aspect="AspectFit"
       HeightRequest="200" /
```

Alternatively, `OnAppTheme<T>` resources can be consumed by implicit or explicit [`Style`](xref:Xamarin.Forms.Style) objects:

```xaml
<Style x:Key="imageLogoStyle"
       TargetType="Image">
    <Setter Property="Source"
            Value="{DynamicResource ImageLogo}" />
    <Setter Property="Aspect"
            Value="AspectFit" />
</Style>
```

> [!IMPORTANT]
> `OnAppTheme<T>` resources should be consumed with the `DynamicResource` markup extension. This ensures that the consuming object's appearance is updated when the system theme changes.

### OnAppTheme markup extension

The `OnAppTheme` markup extension enables you to specify a resource to be consumed, such as an image or color, based on the current system theme. It provides the same functionality as the `OnAppTheme<T>` class, but with a more concise representation:

```xaml
<ContentPage ...>
    <StackLayout Margin="20">
        <Label Text="This text is green in light mode, and red in dark mode."
               TextColor="{OnAppTheme Light=Green, Dark=Red}" />
        <Image Source="{OnAppTheme Light=lightlogo.png, Dark=darklogo.png}" />
    </StackLayout>
</ContentPage>
```

In this example, the text color of the first [`Label`](xref:Xamarin.Forms.Label) is set to green when the device is using its light theme, and is set to red when the device is using its dark theme. Similarly, the [`Image`](xref:Xamarin.Forms.Image) displays a different image file based upon the current system theme.

For more information about the `OnAppTheme` markup extension, see [OnAppTheme markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension).

## Detect the current system theme

The current system theme can be detected by getting the value of the `Application.RequestedTheme` property:

```csharp
OSAppTheme currentTheme = Application.Current.RequestedTheme;
```

The `RequestedTheme` property returns an `OSAppTheme` enumeration member. The `OSAppTheme` enumeration defines the following members:

- `Unspecified`, which indicates that the device is using an unspecified theme.
- `Light`, which indicates that the device is using its light theme.
- `Dark`, which indicates that the device is using its dark theme.

## React to theme changes

The system theme on a device may change for a variety of reasons, depending on how the device is configured. Xamarin.Forms apps can be notified when the system theme changes by handling the `Application.RequestedThemeChanged` event:

```csharp
Application.Current.RequestedThemeChanged += (s, a) =>
{
    // Respond to the theme change
};
```

The `AppThemeChangedEventArgs` object, which accompanies the `RequestedThemeChanged` event, has a single property named `RequestedTheme`, of type `OSAppTheme`. This property can be examined to detect the requested system theme.

## Related links

- [SystemThemes (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-systemthemesdemo/)
- [OnAppTheme markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onapptheme-markup-extension)
- [Resource Dictionaries](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamic Styles in Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Styling Xamarin.Forms Apps using XAML Styles](~/xamarin-forms/user-interface/styles/xaml/index.md)
