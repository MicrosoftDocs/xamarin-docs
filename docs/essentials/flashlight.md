---
title: "Xamarin.Essentials: Flashlight"
description: "This document describes the Flashlight class in Xamarin.Essentials, which has the ability to turn on or off the device's camera flash to turn it into a flashlight."
ms.assetid: 06A03553-D212-43A2-9E6E-C2D2D93EB136
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---

# Xamarin.Essentials: Flashlight

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Flashlight** class has the ability to turn on or off the device's camera flash to turn it into a flashlight.

## Get started

[!include[](~/essentials/includes/get-started.md)]

To access the **Flashlight** functionality the following platform specific setup is required.

# [Android](#tab/android)

The Flashlight and Camera permissions are required and must be configured in the Android project. This can be added in the following ways:

Open the **AssemblyInfo.cs** file under the **Properties** folder and add:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Flashlight)]
[assembly: UsesPermission(Android.Manifest.Permission.Camera)]
```

OR Update Android Manifest:

Open the **AndroidManifest.xml** file under the **Properties** folder and add the following inside of the **manifest** node.

```xml
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.CAMERA" />
```

Or right click on the Android project and open the project's properties. Under **Android Manifest** find the **Required permissions:** area and check the **FLASHLIGHT** and **CAMERA** permissions. This will automatically update the **AndroidManifest.xml** file.

By adding these permissions [Google Play will automatically filter out devices](http://developer.android.com/guide/topics/manifest/uses-feature-element.html#permissions-features) without specific hardware. You can get around this by adding the following to your AssemblyInfo.cs file in your Android project:

```csharp
[assembly: UsesFeature("android.hardware.camera", Required = false)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

# [iOS](#tab/ios)

No additional setup required.

# [UWP](#tab/uwp)

No additional setup required.

-----

## Using Flashlight

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The flashlight can be turned on and off through the `TurnOnAsync` and `TurnOffAsync` methods:

```csharp
try
{
    // Turn On
    await Flashlight.TurnOnAsync();

    // Turn Off
    await Flashlight.TurnOffAsync();
}
catch (FeatureNotSupportedException fnsEx)
{
    // Handle not supported on device exception
}
catch (PermissionException pEx)
{
    // Handle permission exception
}
catch (Exception ex)
{
    // Unable to turn on/off flashlight
}
```

## Platform Implementation Specifics

### [Android](#tab/android)

The Flashlight class has been optimized based on the device's operating system.

#### API Level 23 and Higher

On newer API levels, [Torch Mode](https://developer.android.com/reference/android/hardware/camera2/CameraManager.html#setTorchMode) will be used to turn on or off the flash unit of the device.

#### API Level 22 and Lower

A camera surface texture is created to turn on or off the `FlashMode` of the camera unit. 

### [iOS](#tab/ios)

[AVCaptureDevice](https://developer.xamarin.com/api/type/AVFoundation.AVCaptureDevice/) is used to turn on and off the Torch and Flash mode of the device.

### [UWP](#tab/uwp)

[Lamp](https://docs.microsoft.com/uwp/api/windows.devices.lights.lamp) is used to detect the first lamp on the back of the device to turn on or off.

-----

## API

- [Flashlight source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Flashlight)
- [Flashlight API documentation](xref:Xamarin.Essentials.Flashlight)
