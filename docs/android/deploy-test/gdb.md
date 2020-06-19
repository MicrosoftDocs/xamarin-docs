---
title: "GDB"
ms.prod: xamarin
ms.assetid: CD0BE462-FA38-4881-B481-82AD05B3B8FE
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/05/2018
---

# GDB

## Overview

Xamarin.Android 4.10 introduced partial support for using `gdb` 
by using the `_Gdb` MSBuild target. 

> [!NOTE]
> `gdb` support requires that the
Android NDK be installed.

There are three ways to use `gdb`:

1. [Debug builds with Fast Deployment enabled](#Debug_Builds_with_Fast_Deployment) .
1. [Debug builds with Fast Deployment disabled](#Debug_Builds_without_Fast_Deployment) .
1. [Release builds](#Release_Builds) .

When things go wrong, please see the
[Troubleshooting](#Troubleshooting) section.

<a name="Debug_Builds_with_Fast_Deployment"></a>

### Debug Builds with Fast Deployment

When building and deploying a Debug build with Fast Deployment enabled,
`gdb` can be attached by using the `_Gdb` MSBuild target.

First, install the app. This can be done via the IDE, or via the
command line:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:Install *.csproj
```

Secondly, run the `_Gdb` target. At the end of execution, a `gdb`
command line will be printed:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
...
```

The `_Gdb` target will launch an arbitrary launcher Activity declared
within your `AndroidManifest.xml` file. To explicitly specify which
Activity to run, use the `RunActivity` MSBuild property. Starting
Services and other Android constructs is not supported at this time.

The `_Gdb` target will create a `gdb-symbols` directory
and copy the contents of your target's `/system/lib` and `$APPDIR/lib` directories there.

> [!NOTE]
> The contents of the `gdb-symbols` directory are tied
to the Android target you deployed to, and will not be automatically replaced
should you change the target. (Consider this a bug.) If you change Android target
devices, you will need to manually delete this directory.

Finally, copy the generated `gdb` command and execute it in your
shell:

```bash
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) bt
#0  0x40082e84 in nanosleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#1  0x4008ffe6 in sleep () from /Users/jon/Development/Projects/Scratch.HelloXamarin20/gdb-symbols/libc.so
#2  0x74e46240 in ?? ()
#3  0x74e46240 in ?? ()
(gdb) c
```

<a name="Debug_Builds_without_Fast_Deployment"></a>

## Debug Builds without Fast Deployment

Debug builds *with* Fast Deployment operate by copying the Android
NDK's `gdbserver` program into the Fast Deployment `.__override__`
directory. When Fast Deployment is disabled, this directory may not
exist.

There are two workarounds:

- Set the `debug.mono.log` system property so that the `.__override__` directory is created.
- Include `gdbserver` within your `.apk`.

### Setting the `debug.mono.log` System Property

To set the `debug.mono.log` system property, use the `adb` command:

```bash
$ adb shell setprop debug.mono.log gc
```

Once the system property has been set, execute the `_Gdb` target and
the printed `gdb` command, as with the Debug Builds with Fast
Deployment configuration:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```

### Including `gdbserver` in your app

To include `gdbserver` within your app:

1. Find `gdbserver` within your Android NDK (it should be in
   **$ANDROID\_NDK\_PATH/prebuilt/android-arm/gdbserver/gdbserver**), and
   copy it into your Project directory.

2. Rename `gdbserver` to **libs/armeabi-v7a/libgdbserver.so**.

3. Add **libs/armeabi-v7a/libgdbserver.so** to your Project with a
   **Build action** of `AndroidNativeLibrary`.

4. Rebuild and reinstall your application.

Once the app has been reinstalled, execute the `_Gdb` target and the
printed `gdb` command, as with the Debug Builds with Fast Deployment
configuration:

```bash
$ /Library/Frameworks/Mono.framework/Commands/xbuild /t:_Gdb *.csproj
  ...
    Target _Gdb:
        "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
  ...
$ "/opt/android/ndk/toolchains/arm-linux-androideabi-4.4.3/prebuilt/darwin-x86/bin/arm-linux-androideabi-gdb" -x "/Users/jon/Development/Projects/Scratch.HelloXamarin20//gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
...
(gdb) c
```

<a name="Release_Builds"></a>

## Release Builds

`gdb` support requires three things:

1. The `INTERNET` permission.
2. App Debugging enabled.
3. An accessible `gdbserver`.

The `INTERNET` permission is enabled by default in Debug apps. If it is
not already present in your application, you may add it either by
editing **Properties/AndroidManifest.xml** or by editing the
[Project Properties](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest).

App debugging can be enabled by either setting the
[ApplicationAttribute.Debugging](xref:Android.App.ApplicationAttribute.Debuggable)
custom attribute property to `true`, or by editing
**Properties/AndroidManifest.xml** and setting the
`//application/@android:debuggable` attribute to `true`:

```xml
<application android:label="Example.Name.Here" android:debuggable="true">
```

An accessible `gdbserver` may be provided by following the
[Debug Builds without Fast Deployment](#Debug_Builds_without_Fast_Deployment) section.

One wrinkle: The `_Gdb` MSBuild target will kill any previously running
app instances. This will not work on pre-Android v4.0 targets.

<a name="Troubleshooting"></a>

## Troubleshooting

### `mono_pmip` doesn't work

The `mono_pmip` function (useful for
[obtaining managed stack frames](https://www.mono-project.com/docs/debug+profile/debug/#debugging-with-gdb)) 
is exported from `libmonosgen-2.0.so`, which the `_Gdb` target does not
currently pull down. (This will be fixed in a future release.)

To enable calling functions located in `libmonosgen-2.0.so`, copy it
from the target device into the `gdb-symbols` directory:

```bash
$ adb pull /data/data/Mono.Android.DebugRuntime/lib/libmonosgen-2.0.so Project/gdb-symbols
```

Then restart your debugging session.

### Bus error: 10 when running the `gdb` command

When the `gdb` command errors out with `"Bus error: 10"`, restart the
Android device.

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
Bus error: 10
$
```

### No stack trace after attach

```bash
$ "/path/to/arm-linux-androideabi-gdb" -x "Project/gdb-symbols/gdb.env"
GNU gdb (GDB) 7.3.1-gg2
Copyright (C) 2011 Free Software Foundation, Inc.
...
(gdb) bt
No stack.
```

This is usually a sign that the contents of the `gdb-symbols` directory
are not synchronized with your Android target. (Did you change your
Android target?)

Please delete the `gdb-symbols` directory and try again.
