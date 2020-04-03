---
title: "Detect dark mode in Xamarin.Forms applications"
description: "Dark mode can be supported in any Xamarin.Forms application using a combination of ResourceDictionaries, DynamicResources, and platform knowledge."
ms.prod: xamarin
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 03/13/2020
---

# Detect dark mode in Xamarin.Forms applications

Xamarin.Forms applications can respond to operating system theme changes by using the same strategy that enables you to support [theming](theming.md). The main difference is in how the change of theme is triggered. Dark mode refers to a broader set of appearance preferences that can be set at the operating system level, and which applications are expected to respect and respond to immediately.

The minimum requirements for using dark mode in your Xamarin.Forms applications are:

- iOS 13 or greater.
- Android 9 or greater (Android 9 supports appearance modes that you can query).
- Android 10 or greater (Android 10 notifies apps of mode changes).
- UWP v10.0.10240.0 or greater.
- Xamarin.Forms (any version).

The process of supporting appearance modes, including light and dark, is as follows:

1. Define resources shared by your entire application in a [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary).
2. Define a resource dictionary theme for each appearance mode that provides overrides specific to each style you wish to change.
3. Set the default appearance mode theme in your application's **App.xaml** file.
4. Style your application using the `DynamicResource` markup extension where you want styles to react when appearance modes change.
5. Add code to listen for OS theme changes, and load your application's corresponding theme.

The following screenshots show themed pages, for light and dark mode:

[![Screenshot of the main page of a themed app, on iOS and Android](theming-images/main-page-both-themes.png "Main page of themed app")](theming-images/main-page-both-themes-large.png#lightbox "Main page of themed app")
[![Screenshot of the detail page of a themed app, on iOS and Android](theming-images/detail-page-both-themes.png "Detail page of themed app")](theming-images/detail-page-both-themes-large.png#lightbox "Detail page of themed app")

## Define themes

Follow the [theming guide](theming.md) for step by step details about creating dark and light themes.

You can easily set a new theme for your application in the appropriate location of your platform code. To load a new theme, simply replace the application's current resource dictionary with a themed resource dictionary of your choice:

```csharp
App.Current.Resources = new YourDarkTheme();
```

## Detect and apply theme

Detecting the currently running theme can be achieved by using the [`RequestedTheme`](~/essentials/app-theme.md) feature of [Xamarin.Essentials](~/essentials/index.md) (version 1.4.0 or newer). You can then create a helper method in a new class or in the `App` class to configure the theme:

```csharp
public partial class App : Application
{
    public static void ApplyTheme()
    {
        if (AppInfo.RequestedTheme == AppTheme.Dark)
        {
            // Change to dark theme
            // e.g. App.Current.Resources = new YourDarkTheme();
        }
        else
        {
            // Change to light theme
            // e.g. App.Current.Resources = new YourLightTheme();
        }
    }
}
```

## React to appearance mode changes

The appearance mode on a device may change for a variety of reasons depending on how the user has configured their preferences including explicitly choosing a mode, time of day, or environmental factors such as low-light. You'll have to add platform code to ensure your application can react to these changes, and the following sections discuss this in more detail.

### Android

To support dark mode in your app you must update your app's theme, that can be found in `Resources/values/styles.xml`, to inherit from a `DayNight` theme:

```xml
<style name="MainTheme.Base" parent="Theme.AppCompat.DayNight">
```

If you have upgraded to AndroidX's [Material Components](https://www.nuget.org/packages/Xamarin.Google.Android.Material/) (1.1.0-rc2) or newer you can use:

```xml
<style name="MainTheme.Base" parent="Theme.MaterialComponents.DayNight">
```

In your application's **MainActivity.cs** file, add the `ConfigChanges.UiMode` field to the `ConfigurationChanges` property in the `Activity` attribute, so that your app will be notified of UI mode changes:

```csharp
[Activity(
    // other settings
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode)]
```

In the same **MainActivity.cs** file, override the `OnConfigurationChanged` method:

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);
    App.ApplyTheme();
}
```

### iOS

On iOS, appearance mode changes are notified on the UIViewController, which in Xamarin.Forms is the `ContentPage`. In order to override that method, create a custom renderer in your iOS project called `PageRenderer.cs`:

```csharp
using System;
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(ContentPage), typeof(YourApp.iOS.Renderers.PageRenderer))]
namespace YourApp.iOS.Renderers
{
    public class PageRenderer : Xamarin.Forms.Platform.iOS.PageRenderer
    {
        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                App.ApplyTheme();
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"\t\t\tERROR: {ex.Message}");
            }
        }

        public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
        {
            base.TraitCollectionDidChange(previousTraitCollection);

            App.ApplyTheme();
        }
    }
}
```

Overriding the `TraitCollectionDidChange` method enables you to act on a `UserInterfaceStyle` change.

### UWP

On UWP, add the following code to your application's **MainPage.xaml.cs** file:

```csharp
public sealed partial class MainPage
{
    UISettings uiSettings;

    public MainPage()
    {
        this.InitializeComponent();

        LoadApplication(new YourApp.App());

        uiSettings = new UISettings();
        uiSettings.ColorValuesChanged += ColorValuesChanged;
    }

    private void ColorValuesChanged(UISettings sender, object args)
    {
        Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
        {
            App.ApplyTheme();
        });
    }
}
```

## Related links

- [Theming (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [Resource Dictionaries](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamic Styles in Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Styling Xamarin.Forms Apps using XAML Styles](~/xamarin-forms/user-interface/styles/xaml/index.md)
