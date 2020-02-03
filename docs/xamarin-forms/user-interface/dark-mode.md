---
title: "SUpporting Dark Mode in a Xamarin.Forms Application"
description: "Dark Mode can easily be supporting in any Xamarin.Forms application using a combination of ResourceDictionaries, DynamicResources, and platform knowledge."
ms.date: 02/01/2020
---

# Dark Mode in a Xamarin.Forms Application

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)

Xamarin.Forms applications can respond to operating system theme changes by using the same strategy that enables you to support [theming](theming.md). The main difference is in how the change of theme is triggered. Dark Mode refers to a broader set of display appearance preferences that can be set at the operating system level, and which applications are expected to respect and respond to immediately. 

Minimum Requirements:

* iOS 13 or greater
* Android 9 supports appearance modes that you can query
* Android 10 notifies apps of mode changes
* UWP v10.0.10240.0 or greater
* Xamarin.Forms any version

The process of supporting appearance modes, including dark and light, is as follows:

1. Define resources shared by your entire application in a [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary).
2. Define a resource dictionary theme for each appearance mode that provides overrides specific to each style you wish to change.
3. Set the default appearance mode theme in your application's **App.xaml** file.
4. Style your application using the `DynamicResource` markup extension where you want styles to react when appearance modes change.
5. Add code to listen for OS theme changes, and load your application's corresponding theme.

The following screenshots show themed pages, one for dark mode and another for light mode:

[![Screenshot of the main page of a themed app, on iOS and Android](theming-images/main-page-both-themes.png "Main page of themed app")](theming-images/main-page-both-themes-large.png#lightbox "Main page of themed app")
[![Screenshot of the detail page of a themed app, on iOS and Android](theming-images/detail-page-both-themes.png "Detail page of themed app")](theming-images/detail-page-both-themes-large.png#lightbox "Detail page of themed app")

## Define themes

Follow our [theming guide](theming.md) for step by step details about creating dark and light themes.

## Reacting to appearance mode changes

The appearance mode on a device may change for a variety of reasons depending on how the user has configured their preferences including explicitly choosing a mode, time of day, or environmental factors such as low-light. To make sure you're application can react to those changes, you'll want to add a bit of platform code such as the following:

# [Android](#tab/android)

Add `ConfigChanges.UiMode` to `ConfigurationChanges` in your application's **MainActivity.cs** so your app can be notified of changes.

```csharp
[Activity(
    // other settings
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode)]
```

In the same **MainActivity.cs** file, override the `OnConfigurationChanged` method.

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);

    if ((newConfig.UiMode & UiMode.NightNo) != 0)
    {
        // change to light
    }
    else
    {
        // change to dark
    }
}
```

# [iOS](#tab/ios)

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
                SetAppTheme();
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"\t\t\tERROR: {ex.Message}");
            }
        }

        public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
        {
            base.TraitCollectionDidChange(previousTraitCollection);
            
            if(this.TraitCollection.UserInterfaceStyle != previousTraitCollection.UserInterfaceStyle)
            {
                SetAppTheme();
            }

            
        }

        void SetAppTheme()
        {
            
            if (this.TraitCollection.UserInterfaceStyle == UIUserInterfaceStyle.Dark)
            {
                // change to dark
            }
            else
            {
                // change to light
            }
        
        }
    }
}
```

Overriding the `TraitCollectionDidChange` method enables you to act on a `UserInterfaceStyle` change.

# [UWP](#tab/uwp)

Add the following code to your application's **MainPage.xaml.cs** file:

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
        var backgroundColor = sender.GetColorValue(UIColorType.Background);
        var isDarkMode = backgroundColor == Colors.Black;
        if(isDarkMode)
        {
            Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
            {
                // change to dark
            });
            
        }
        else
        {
            Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
            {
                // change to light
            });
        }
    }
}
```

## Setting dark and light themes

After following the [theming](theming.md) guide, you can easily set a new theme for your application in the appropriate location of the platform code above. To load a new theme, simply replace the application's current resource dictionary like this:

```csharp
App.Current.Resources = new YourDarkTheme();
```

## Related links

- [Theming (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [Resource Dictionaries](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamic Styles in Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Styling Xamarin.Forms Apps using XAML Styles](~/xamarin-forms/user-interface/styles/xaml/index.md)
