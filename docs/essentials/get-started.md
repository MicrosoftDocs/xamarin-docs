---
title: "Get Started with Xamarin.Essentials"
description: "Xamarin.Essentials provides a single cross-platform API that works with any iOS, Android, or UWP application that can be accessed from shared code no matter how the user interface is created."
ms.assetid: B2669C48-B659-4854-BD80-FEB0E876F5B9
author: jamesmontemagno
ms.author: jamont
ms.custom: video
ms.date: 07/10/2019
---

# Get Started with Xamarin.Essentials

Xamarin.Essentials provides a single cross-platform API that works with any iOS, Android, or UWP application that can be accessed from shared code no matter how the user interface is created. See the [platform & feature support guide](platform-feature-support.md) for more information on supported operating systems.

## Installation

Xamarin.Essentials is available as a NuGet package that can be added to any existing or new project using Visual Studio.

1. Download and install [Visual Studio](https://visualstudio.microsoft.com/) with the [Visual Studio tools for Xamarin](~/get-started/installation/index.md).

2. Open an existing project, or create a new project using the Blank App template under **Visual Studio C#** (Android, iPhone & iPad, or Cross-Platform).

    > [!IMPORTANT]
    > If adding to a UWP project ensure Build 16299 or higher is set in the project properties.

3. Add the [**Xamarin.Essentials**](https://www.nuget.org/packages/Xamarin.Essentials/) NuGet package to each project:

    # [Visual Studio](#tab/windows)

    In the Solution Explorer panel, right click on the solution name and select **Manage NuGet Packages**. Search for **Xamarin.Essentials** and install the package into **ALL** projects including Android, iOS, UWP, and .NET Standard libraries.

    # [Visual Studio for Mac](#tab/macos)

    In the Solution Explorer panel, right click on the project name and select **Add > Add NuGet Packages...**. Search for **Xamarin.Essentials** and install the package into **ALL** projects including Android, iOS, and .NET Standard libraries.

    -----

4. Add a reference to Xamarin.Essentials in any C# class to reference the APIs.

    ```csharp
    using Xamarin.Essentials;
    ```

5. Xamarin.Essentials requires platform-specific setup:

    # [Android](#tab/android)

    Xamarin.Essentials supports a minimum Android version of 4.4, corresponding to API level 19, but the target Android version for compiling must be 9.0, corresponding to API level 28. (In Visual Studio, these two versions are set in the Project Properties dialog for the Android project, in the Android Manifest tab. In Visual Studio for Mac, they're set in the Project Options dialog for the Android project, in the Android Application tab.)

    Xamarin.Essentials installs version 28.0.0.1 of the Xamarin.Android.Support libraries that it requires. Any other Xamarin.Android.Support libraries that your application requires should also be updated to version 28.0.0.1 using the NuGet package manager. All Xamarin.Android.Support libraries used by your application should be the same, and should be at least version 28.0.0.1. Refer to the [troubleshooting page](troubleshooting.md) if you have issues adding the Xamarin.Essentials NuGet or updating NuGets in your solution.

    In the Android project's `MainLauncher` or any `Activity` that is launched Xamarin.Essentials must be initialized in the `OnCreate` method:

    ```csharp
    protected override void OnCreate(Bundle savedInstanceState) {
        //...
        base.OnCreate(savedInstanceState);
        Xamarin.Essentials.Platform.Init(this, savedInstanceState); // add this line to your code, it may also be called: bundle
        //...
    ```

    To handle runtime permissions on Android, Xamarin.Essentials must receive any `OnRequestPermissionsResult`. Add the following code to all `Activity` classes:

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Android.Content.PM.Permission[] grantResults)
    {
        Xamarin.Essentials.Platform.OnRequestPermissionsResult(requestCode, permissions, grantResults);

        base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
    }
    ```

    # [iOS](#tab/ios)

    No additional setup required.

    # [UWP](#tab/uwp)

    No additional setup required.

    -----

6. Follow the [Xamarin.Essentials guides](index.md) that enable you to copy and paste code snippets for each feature.

## Xamarin.Essentials - Cross-Platform APIs for Mobile Apps (video)

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Snack-Pack-XamarinEssentials-Cross-Platform-APIs-for-Mobile-Apps/player]

## Other Resources

We recommend developers new to Xamarin visit [getting started with Xamarin development](~/cross-platform/getting-started/index.md).

Visit the [Xamarin.Essentials GitHub Repository](https://github.com/xamarin/Essentials) to see the current source code, what is coming next, run samples, and clone the repository. Community contributions are welcome!

Browse through the [API documentation](xref:Xamarin.Essentials) for every feature of Xamarin.Essentials.
