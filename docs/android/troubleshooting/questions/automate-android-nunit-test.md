---
title: "How do I automate an Android NUnit Test project?"
ms.topic: article
ms.prod: xamarin
ms.assetid: EA3CFCC4-2D2E-49D6-A26C-8C0706ACA045
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
---

# How do I automate an Android NUnit Test project?

> [!NOTE]
> This guide covers steps for setting up an Android
NUnit test project, not a Xamarin.UITest project. Xamarin.UITest guides
can be found [here](https://docs.microsoft.com/appcenter/test-cloud/preparing-for-upload/uitest).

When you create an Android Unit Test Project
[Visual Studio for Mac] or Unit Test App (Android)
[Visual Studio], by default it will not automatically run your tests.
To automate your android Unit Test: To run NUnit tests on a target
device, we use an `Android.App.Instrumentation` subclass, which can be
created and executed by using the `adb shell am instrument` command.

First, we create the **TestInstrumentation.cs** file, which creates a
subclass of `Xamarin.Android.NUnitLite.TestSuiteInstrumentation`
(declared in `Xamarin.Android.NUnitLite.dll`). The
`TestInstrumentation(IntPtr, JniHandleOwnership)` constructor _must_ be
provided, and the virtual `AddTests()` method must be overridden.
`AddTests()` controls which tests are actually executed. This file is
largely boilerplate.

Next, the `.csproj` must be modified to add `TestInstrumentation.cs`.

Optionally, the `.csproj` may be modified to add the `RunTests` MSBuild
target, which would allow invoking the unit tests as:

```shell
msbuild /t:RunTests Project.csproj
```

Using a new target is not required; the corresponding adb command may
be used instead:

```shell
adb shell am instrument -w @PACKAGE_NAME@/app.tests.TestInstrumentation
```

Replace `@PACKAGE\_NAME@` as appropriate; it is the value present in the
**AndroidManifest.xml** `/manifest/@package` attribute.

*Important Note*: With the
[Xamarin.Android 5.0](https://developer.xamarin.com/releases/android/xamarin.android_5/xamarin.android_5.1/#Android_Callable_Wrapper_Naming)
release, the default package names for Android Callable Wrappers will
be based on the MD5SUM of the assembly-qualified name of the type being
exported. This allows the same fully-qualified name to be provided from
two different assemblies and not get a packaging error. So make sure
that you use the \`Name\` property on the \`Instrumentation\` attibute
to generate a readable ACW/class name.

_The ACW name must be used in the `adb` command_. Renaming/refactoring
the C# class will thus require modifying the `RunTests` command to use
the correct ACW name.

Additions to the .csproj file:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Project DefaultTargets="Build" ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <ItemGroup>
        <Compile Include="TestInstrumentation.cs" />
    </ItemGroup>
    <Target Name="RunTests" DependsOnTargets="_ValidateAndroidPackageProperties">
        <Exec Command="&quot;$(_AndroidPlatformToolsDirectory)adb&quot; $(AdbTarget) $(AdbOptions) shell am instrument -w $(_AndroidPackage)/app.tests.TestInstrumentation" />
    </Target>
</Project>
```

**TestInstruments.cs**:

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

