# [Visual Studio](#tab/vswin)

To complete this tutorial you should have Visual Studio 2019 (latest release), with the **Mobile development with .NET** workload installed. In addition, you will require a paired Mac to build the tutorial application on iOS. For information about installing the Xamarin platform, see [Installing Xamarin](~/get-started/installation/index.md). For information about connecting Visual Studio 2019 to a Mac build host, see [Pair to Mac for Xamarin.iOS development](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Launch Visual Studio, and create a new blank Xamarin.Forms app named **AppLifecycleTutorial**.

    > [!IMPORTANT]
    > The C# and XAML snippets in this tutorial requires that the solution is named **AppLifecycleTutorial**. Using a different name will result in build errors when you copy code from this tutorial into the solution.

    For more information about the .NET Standard library that gets created, see [Anatomy of a Xamarin.Forms application](~/get-started/first-app/index.md) in the [Xamarin.Forms Quickstart Deep Dive](~/get-started/first-app/index.md).

1. In **Solution Explorer**, in the **AppLifecycleTutorial** project, expand **App.xaml** and double-click **App.xaml.cs** to open it. Then, in **App.xaml.cs**, update the `OnStart`, `OnSleep`, and `OnResume` overrides as follows:

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    This code updates the application lifecycle method overrides, with `Console.WriteLine` statements that indicate when each method has been invoked:

    - The `OnStart` method is invoked when the application starts.
    - The `OnSleep` method is invoked when the application goes to the background.
    - The `OnResume` method is invoked when the application resumes from the background.

    > [!NOTE]
    > There is no method for application termination. Under normal circumstances, application termination will occur from the `OnSleep` method.

1. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator. When the application starts, the `OnStart` method is invoked, and **OnStart** is output to the Visual Studio **Output** window:

    ```
    [Mono] Found as 'java_interop_jnienv_get_object_array_element'.
    OnStart
    [OpenGLRenderer] HWUI GL Pipeline
    ```

    When the application is backgrounded (by tapping the Home button on iOS or Android), the `OnSleep` method is invoked:

    ```
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    OnSleep
    [Mono] Image addref System.Runtime.Serialization[0x83ee19c0] -> System.Runtime.Serialization.dll[0x83f57b00]: 2
    ```

    Then, when the application resumes from the background (tap the application icon on iOS, tap the Overview button on Android and select the AppLifecycleTutorial application), the `OnResume` method is invoked:

    ```
    Thread finished: <Thread Pool> #5
    OnResume
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    ```

    > [!NOTE]
    > These code blocks show example output when running the application on Android.

    In Visual Studio, stop the application.

    For more information about the Xamarin.Forms app lifecycle, see [Xamarin.Forms App Lifecycle](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

# [Visual Studio for Mac](#tab/vsmac)

To complete this tutorial you should have Visual Studio for Mac (latest release), with iOS and Android platform support installed. In addition, you will also require Xcode (latest release). For more information about installing the Xamarin platform, see [Installing Xamarin](~/get-started/installation/index.md).

1. Launch Visual Studio for Mac, and create a new blank Xamarin.Forms app named **AppLifecycleTutorial**.

    > [!IMPORTANT]
    > The C# and XAML snippets in this tutorial requires that the solution is named **AppLifecycleTutorial**. Using a different name will result in build errors when you copy code from this tutorial into the solution.

    For more information about the .NET Standard library that gets created, see [Anatomy of a Xamarin.Forms application](~/get-started/first-app/index.md) in the [Xamarin.Forms Quickstart Deep Dive](~/get-started/first-app/index.md).

1. In **Solution Pad**, in the **AppLifecycleTutorial** project, expand **App.xaml** and double-click **App.xaml.cs** to open it. Then, in **App.xaml.cs**, update the `OnStart`, `OnSleep`, and `OnResume` overrides as follows:

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    This code updates the application lifecycle method overrides, with `Console.WriteLine` statements that indicate when each method has been invoked:

    - The `OnStart` method is invoked when the application starts.
    - The `OnSleep` method is invoked when the application goes to the background.
    - The `OnResume` method is invoked when the application resumes from the background.

    > [!NOTE]
    > There is no method for application termination. Under normal circumstances, application termination will occur from the `OnSleep` method.

1. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator. When the application starts, the `OnStart` method is invoked, and **OnStart** is output to the Visual Studio for Mac **Application Output** window:

    ```
    2019-02-11 12:05:23.164761+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:05:23.165297+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    2019-02-11 12:05:23.685430+0000 AppLifecycleTutorial.iOS[4089:361037] OnStart
    ```

    When the application is backgrounded (by tapping the Home button on iOS or Android), the `OnSleep` method is invoked:

    ```
    2019-02-11 12:06:12.394301+0000 AppLifecycleTutorial.iOS[4089:361037] OnSleep
    Thread started: <Thread Pool> #3
    Thread started: <Thread Pool> #4
    Thread started: <Thread Pool> #5
    Thread started: <Thread Pool> #6
    2019-02-11 12:06:13.056968+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:06:13.057264+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    ```

    Then, when the application resumes from the background (tap the application icon on iOS, tap the Overview button on Android and select the AppLifecycleTutorial application), the `OnResume` method is invoked:

    ```
    2019-02-11 12:07:10.321905+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4130
    2019-02-11 12:07:10.322557+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskCopyDebugDescription: AppLifecycleTuto[4130]/0#-1 LF=0
    2019-02-11 12:07:17.110938+0000 AppLifecycleTutorial.iOS[4130:365695] OnResume
    ```

    > [!NOTE]
    > These code blocks show example output when running the application on iOS.

    In Visual Studio for Mac, stop the application.

    For more information about the Xamarin.Forms app lifecycle, see [Xamarin.Forms App Lifecycle](~/xamarin-forms/app-fundamentals/app-lifecycle.md).
