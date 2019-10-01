---
title: "Xamarin.Forms Android splash screen"
description: "This article explains how to create a splash screen in a Xamarin.Forms Android application."
ms.prod: xamarin
ms.assetId: 338C8F60-90F2-4B3D-BB47-7F846AE8DC6C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/1/2019
---

# Xamarin.Forms Android splash screen

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-splashscreen/)

Most Android applications have a delay while an application is launched. The default behavior is typically to show a blank screen while the application prepares to launch. However, many developers want to offer a custom launch, or splash screen, experience for their users. This article explains how to configure an Android application to display custom UI during the launch process.

[![Custom splash screen on Android](splashscreen-images/android-splashscreen-cropped.png)](splashscreen-images/android-splashscreen.png#lightbox)

> [!NOTE]
> This article explains how to create a splash screen in the context of a Xamarin.Forms application but the steps followed here are Android platform features and not unique to Xamarin.Forms.

## Create a drawable resource

The splash screen will need a drawable resource to display during the launch process. The sample application defines a **splash.xml** file in the **Resources/drawable** folder:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android"
            android:opacity="opaque">

  <item android:drawable="@color/colorSplash" />
  <item>
    <bitmap android:src="@drawable/xamagon"
            android:tileMode="disabled"
            android:gravity="center" />
  </item>
  
</layer-list>
```

This file uses a `layer-list` to define layers of drawables. Layer lists are drawn in array order, with the highest index item being drawn last.

This `layer-list` instance defines a solid color layer. The `@color/colorSplash` value must be defined in the **Resources/values/colors.xml** file:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
  ...
  <color name="colorSplash">#3498DB</color>
</resources>
```

The `layer-list` instance also defines a bitmap image. The sample project includes a file called **xamagon.png** in the **drawable** folder that has a **Build Action** of `AndroidResource`, allowing it to be referenced as a drawable by the **splash.xml** file.

For more about drawables on Android, see [Android drawable resource documentation](https://developer.android.com/guide/topics/resources/drawable-resource)

## Create a custom theme

The splash screen will utilize a custom theme during the launch process to display the **splash.xml** drawable. The following style is defined in the **Resources/values/styles.xml** file:

```xml
...
<style name="SplashScreen" parent="MainTheme.Base">
    <item name="android:windowBackground">@drawable/splash</item>
</style>
...
```

This custom style is based on the **MainTheme.Base** theme, but sets the **android:windowBackground** item to reference the custom drawable.

For more about styles and themes on Android, see [Android styles and themes documentation](https://developer.android.com/guide/topics/ui/look-and-feel/themes)

## Apply custom theme to MainActivity

By default, the `MainActivity` class in Xamarin.Forms is the entry point for a Xamarin.Forms application on Android. The `MainActivity` is prefixed with multiple attributes. To specify the custom splash screen theme, the `Theme` attribute should be customized and the `MainLauncher` attribute must be `true`:

```csharp
[Activity(Label = "FormsSplashDemo",
        Icon = "@mipmap/icon",
        Theme = "@style/SplashScreen",
        MainLauncher = true,
        ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        // ...
    }
```

The `Theme` attribute references the custom `SplashScreen` style defined earlier in the **styles.xml** file.

Once the application has finished starting up, the temporary `SplashScreen` theme needs to be replaced with the theme used during normal application use. Otherwise the application will use the custom **splash.xml** drawable as its background. In the `OnCreate` method, the normal theme is restored:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    SetTheme(Resource.Style.MainTheme_Base);
    // ...
}
```

> [!NOTE]
> If `SetTheme` is called after the `base.OnCreate` method call, it will not be applied. Ensure that the normal theme is reset prior to calling `base.OnCreate`.

Once the theme is defined on the **MainActivity**, running the application will display the custom splash screen while the application starts up.

## Related links

- [Splash screen sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-splashscreen/)]
- [Android styles and themes documentation](https://developer.android.com/guide/topics/ui/look-and-feel/themes)
- [Android drawable resource documentation](https://developer.android.com/guide/topics/resources/drawable-resource)
