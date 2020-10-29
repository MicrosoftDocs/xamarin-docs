---
title: "How do I automate an Android NUnit Test project?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/29/2018
---

# How do I automate an Android NUnit Test project?

> [!NOTE]
> This guide explains how to automate an Android
NUnit test project, not a Xamarin.UITest project. Xamarin.UITest guides
can be found [here](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest).

When you create a **Unit Test App (Android)** project in Visual Studio
(or **Android Unit Test** project in Visual Studio for Mac), this
project will not automatically run your tests by default.
To run NUnit tests on a target device, you can create an
[Android.App.Instrumentation](xref:Android.App.Instrumentation)
subclass that is started by using the following command:

```shell
adb shell am instrument
```

The following steps explain this process:

1. Create a new file called **TestInstrumentation.cs**:

    ```cs
    using System;
    using System.Reflection;
    using Android.App;
    using Android.Content;
    using Android.Runtime;
    using Xamarin.Android.NUnitLite;

    namespace App.Tests {

        [Instrumentation(Name="app.tests.TestInstrumentation")]
        public class TestInstrumentation : TestSuiteInstrumentation {

            public TestInstrumentation (IntPtr handle, JniHandleOwnership transfer) : base (handle, transfer)
            {
            }

            protected override void AddTests ()
            {
                AddTest (Assembly.GetExecutingAssembly ());
            }
        }
    }
    ```

    In this file, `Xamarin.Android.NUnitLite.TestSuiteInstrumentation`
    (from **Xamarin.Android.NUnitLite.dll**) is subclassed to create `TestInstrumentation`.

2. Implement the `TestInstrumentation` constructor and the
    `AddTests` method. The `AddTests` method controls which tests are actually executed.

3. Modify the `.csproj` file to add **TestInstrumentation.cs**. For example:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        ...
        <ItemGroup>
            <Compile Include="TestInstrumentation.cs" />
        </ItemGroup>
        <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
            <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
        </Target>
        ...
    </Project>
    ```

4. Deploy your application in debug or release mode, then stop it.

5. Use the following command to run the unit tests. Replace
    `PACKAGE_NAME` with the app's package name (the package name can be
    found in the app's `/manifest/@package` attribute located
    in **AndroidManifest.xml**):

    ```shell
    adb shell am instrument -w PACKAGE_NAME/app.tests.TestInstrumentation
    ```

6. Optionally, you can modify the `.csproj` file to add the `RunTests`
    MSBuild target. This makes it possible to invoke the unit tests
    with a command like the following:

    ```shell
    msbuild /t:RunTests Project.csproj
    ```

    (Note that using this new target is not required; the earlier `adb` command
    can be used instead of `msbuild`.)

For more information about using the `adb shell am instrument` command
to run unit tests, see the Android Developer
[Running tests with ADB](https://developer.android.com/studio/test/command-line.html#RunTestsDevice)
topic.

> [!NOTE]
> With the
[Xamarin.Android 5.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming)
release, the default package names for Android Callable Wrappers will
be based on the MD5SUM of the assembly-qualified name of the type being
exported. This allows the same fully-qualified name to be provided from
two different assemblies and not get a packaging error. So make sure
that you use the `Name` property on the `Instrumentation` attribute
to generate a readable ACW/class name.

_The ACW name must be used in the `adb` command above_.
Renaming/refactoring the C# class will thus require modifying the
`RunTests` command to use the correct ACW name.
