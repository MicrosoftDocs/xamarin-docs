---
title: "Using Native Libraries"
ms.service: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
description: Learn how to use, debug, and deploy Native Libaries via the standard PInvoke mechanism on Xamarin.Android.
---

# Using Native Libraries

Xamarin.Android supports the use of native libraries via the standard
PInvoke mechanism. You can also bundle additional native libraries which are not
part of the OS into your .apk.

To deploy a native library with a Xamarin.Android application, add the
library binary to the project and set its **Build Action** to **AndroidNativeLibrary**.

To deploy a native library with a Xamarin.Android library project, add the
library binary to the project and set its **Build Action** to **EmbeddedNativeLibrary**.

Note that since Android supports multiple Application Binary Interfaces
(ABIs), Xamarin.Android must know which ABI the native library is built for.
There are two ways this can be done:

1. Path "sniffing"
1. By using an  `AndroidNativeLibrary/Abi` element within the project file

With path sniffing, the parent directory name of the native library is used
to specify the ABI that the library targets. Thus, if you add `lib/armeabi/libfoo.so` to the project, then the ABI will be
"sniffed" as `armeabi`.

Alternatively, you can edit your project file to explicitly specify the ABI
to use:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

For more information about using native libraries, see
[Interop with native libraries](https://www.mono-project.com/docs/advanced/pinvoke/).

## Debugging Native Code with Visual Studio

If you're using *Visual Studio 2019* or *Visual Studio 2017*, you don't have to modify your project files as described above.
You can build and debug C++ inside your Xamarin.Android solution by adding a project reference to
a C++ **Dynamic Shared Library (Android)** project.

To debug native C++ code in your project, follow these steps:

1. Double-click project **Properties** and select the **Android Options** page.
2. Scroll down to **Debugging options**.
3. In the **Debugger** dropdown menu, select **C++** (instead of the default **.NET (Xamarin)**).

Visual Studio C++ developers can see the [SanAngeles_NativeDebug](/samples/xamarin/monodroid-samples/sanangeles-ndk)
sample to try debugging C++ from Visual Studio 2019 or Visual Studio 2017 with Xamarin; and refer to our [blog post](https://devblogs.microsoft.com/xamarin/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) for more information.

## Related Links

- [SanAngeles_NativeDebug (sample)](/samples/xamarin/monodroid-samples/sanangeles-ndk)
- [Developing Xamarin Android Native Applications](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
