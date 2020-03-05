---
title: "Build your first Xamarin.Forms app"
description: "Video guide showing how to build your first Xamarin.Forms application in Visual Studio."
zone_pivot_groups: platform-dev16
ms.prod: xamarin
ms.assetid: 72B6AF82-4D98-47E5-AB54-0A35B3253468
ms.technology: xamarin-forms
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 05/23/2019
---
# Build your first Xamarin.Forms App

_Watch this video and follow along to create your first mobile app with Xamarin.Forms._

::: zone pivot="windows"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-Android-App-with-Visual-Studio-2019-and-Xamarin/player]

## Step-by-step instructions for Windows

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

Follow these steps along with the video above:

1. Choose **File > New > Project...** or press the **Create new project...** button:

    [![Create a new project](images/win-2019/01-sml.png)](images/win-2019/01.png#lightbox)

2. Search for "Xamarin" or choose **Mobile** from the **Project type** menu. Select the **Mobile App (Xamarin.Forms)** project type:

    [![Filter for Xamarin projects](images/win-2019/02-sml.png)](images/win-2019/02.png#lightbox)

3. Choose a project name &ndash; the example uses "AwesomeApp":

    [![Choose a project name](images/win-2019/03-sml.png)](images/win-2019/03.png#lightbox)

4. Click on the **Blank** project type and ensure **Android** and **iOS** are selected:

    [![Android and iOS, with .NET Standard](images/win-2019/04-sml.png)](images/win-2019/04.png#lightbox)

5. Wait until the NuGet packages are restored (a "Restore completed" message will appear in the status bar).

6. New Visual Studio 2019 installations won't have an Android emulator configured. Click the dropdown arrow on the **Debug** button and choose **Create Android Emulator** to launch the emulator creation screen:

    ![Create Android Emulator dropdown](images/win-2019/debug-dropdown.png)

7. In the emulator creation screen, use the default settings and click the **Create** button:

    [![Android emulator creation screen](images/win-2019/create-emulator-sml.png)](images/win-2019/create-emulator.png#lightbox)

8. Creating an emulator will return you to the Device Manager window. Click the **Start** button to launch the new emulator:

    ![Android emulator in the Device Manager](images/win-2019/start-emulator.png)

9. Visual Studio 2019 should now show the name of the new emulator on the **Debug** button:

    ![Android emulator name on the Debug button](images/win-2019/debug-emulator-name.png)

10. Click the **Debug** button to build and deploy the application to the Android emulator:

    ![Android emulator displaying the application](images/win-2019/android-emulator.png)

## Customize the application

The application can be customized to add interactive functionality. Perform the following steps to add user interaction to the application:

1. Edit **MainPage.xaml**, adding this XAML before the end of the `</StackLayout>`:

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

2. Edit **MainPage.xaml.cs**, adding this code to the end of the class:

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

3. Debug the app on Android:

    ![Android app](images/win/07-sml.png)

> [!NOTE]
> The sample application includes the additional interactive functionality that is not covered in the video.

## Build an iOS app in Visual Studio 2019

It's possible to build and debug the iOS app from Visual Studio with a networked Mac computer. Refer to the [setup instructions](~/ios/get-started/installation/windows/index.md) for more information.

This video covers the process of building and testing an iOS app using Visual Studio 2019 on Windows:

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Build-Your-First-iOS-App-with-Visual-Studio-2019-and-Xamarin/player]

::: zone-end
::: zone pivot="win-vs2017"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-Android--iOS-App-in-Visual-Studio-2017/player]

## Step-by-step instructions for Windows

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

Follow these steps along with the video above:

1. Choose **File > New > Project...** or press the **Create new project...** button, then select **Visual C# > Cross-Platform > Mobile App (Xamarin.Forms)**:

    [![Mobile App (Xamarin.Forms)](images/win/01-sml.png)](images/win/01.png#lightbox)

2. Ensure **Android** and **iOS** are selected, with **.NET Standard** code sharing:

    [![Android and iOS, with .NET Standard](images/win/02-sml.png)](images/win/02.png#lightbox)

3. Wait until the NuGet packages are restored (a "Restore completed" message will appear in the status bar).

4. Launch Android emulator by pressing the debug button (or the **Debug > Start Debugging** menu item).

5. Edit **MainPage.xaml**, adding this XAML before the end of the `</StackLayout>`:

    ```xaml
    <Button Text="Click Me" Clicked="Button_Clicked" />
    ```

6. Edit **MainPage.xaml.cs**, adding this code to the end of the class:

    ```csharp
    int count = 0;
    void Button_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. Debug the app on Android:

    ![Android app](images/win/07-sml.png)

    > [!TIP]
    > It is possible to build and debug the iOS app from Visual Studio with a
    > networked Mac computer. Refer to the [setup instructions](~/ios/get-started/installation/windows/index.md)
    > for more information.

::: zone-end
::: zone pivot="macos"

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Building-Your-First-iOS--Android-App-in-Visual-Studio-for-Mac/player]

## Step-by-step instructions for Mac

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/)

Follow these steps along with the video above:

1. Choose **File > New Solution...** or press the **New Project...** button, then select **Multiplatform > App > Blank Forms App**:

    [![Blank Forms App](images/01-sml.png)](images/01.png#lightbox)

2. Ensure **Android** and **iOS** are selected, with **.NET Standard** code sharing:

    [![Android and iOS, with .NET Standard](images/02-sml.png)](images/02.png#lightbox)

3. Restore NuGet packages, by right-clicking on the solution:

    ![Android app](images/03-sml.png)

4. Launch Android emulator by pressing the debug button (or **Run > Start Debugging**).

5. Edit **MainPage.xaml**, adding this XAML before the end of the `</StackLayout>`:

    ```xaml
    <Button Text="Click Me" Clicked="Handle_Clicked" />
    ```

6. Edit **MainPage.xaml.cs**, adding this code to the end of the class:

    ```csharp
    int count = 0;
    void Handle_Clicked(object sender, System.EventArgs e)
    {
        count++;
        ((Button)sender).Text = $"You clicked {count} times.";
    }
    ```

7. Debug the app on Android:

    ![Android app](images/07-sml.png)

8. Right-click to set iOS to the **Startup Project**:

    [![Set the startup project to iOS](images/08-sml.png)](images/08.png#lightbox)

9. Debug the app on iOS:

    ![iOS app](images/09-sml.png)

::: zone-end

You can download the completed code from the [samples gallery](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/getstarted-firstapp/) or view it on [GitHub](https://github.com/xamarin/xamarin-forms-samples/tree/master/GetStarted/FirstApp).

## Next Steps

- [Single Page Quickstart](~/get-started/quickstarts/single-page.md) &ndash; Build a more functional app.
- [Xamarin.Forms Samples](~/xamarin-forms/samples/index.md) &ndash; Download and run code examples and sample apps.
- [Creating Mobile Apps ebook](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) &ndash; In-depth chapters that teach Xamarin.Forms development, available as a PDF and including hundreds of additional samples.
