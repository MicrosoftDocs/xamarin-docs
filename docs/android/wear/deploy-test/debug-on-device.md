---
title: "Debug on a Wear Device"
description: "This article explains how to debug a Xamarin.Android Wear application on a Wear device."
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
---

# Debug on a Wear Device

_This article explains how to debug a Xamarin.Android Wear application on a Wear device._

## Overview

If you have an Android Wear device such as an Android Wear Smartwatch,
You can run the app on the device instead of using an emulator. (If you
are not yet familiar with the process of deploying and running Android
Wear apps, see
[Hello, Wear](~/android/wear/get-started/hello-wear.md).)

## Prepare The Wear Device:

Use the following steps to enable debugging on the Android
Wear device:

1. Open the **Settings** menu on the Android Wear device.

2. Scroll to the bottom of the menu and tap **About**.

3. Tap the build number 7 times.

4. On the **Settings** menu, tap **Developer Options**.

5. Confirm that **ADB debugging** is enabled.

## Debugging over USB

If your Wear device has a USB port, you can connect the Wear device to
your computer, deploy to it, and run/debug the app as you would using
an Android phone (for more information, see
[Debug on a Device](~/android/deploy-test/debugging/debug-on-device.md)).

## Debugging over Bluetooth

If your Wear device does not have a USB port, you can deploy the app to 
the Wear device over Bluetooth by routing the app's debug output to an 
Android phone that is connected to your computer. 

### Prepare Your Phone

Use the following steps to prepare your phone for making Bluetooth
connections to the Wear device: 

1. If you have not already done so, set up your phone for Xamarin.Android development
    as explained in
    [Set Up Device for Development](~/android/get-started/installation/set-up-device-for-development.md).

2. Download and install the free
    [Android Wear](https://play.google.com/store/apps/details?id=com.google.android.wearable.app)
    app from the Google Play Store.

### Connect The Device

Use the following steps to connect your Wear device to your Phone:

1. On the phone that will act as Bluetooth intermediary (configured above), 
    start the Android Wear app. 

2. Tap the **Settings** icon.

3. Enable **Debugging over Bluetooth**. You should see the following status
    displayed on the screen of the Android Wear app:

    ```
    Host: disconnected
    Target: connected
    ```

4. Connect the phone to your computer over USB. On your computer, 
    enter the following commands:

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    If port 4444 is not available, you can use any other available port 
    to which you have access. 

    > [!NOTE]
    > If you restart Visual Studio or Visual Studio for Mac,
    > you must run these commands again to setup a connection to the Wear
    > device.

5. When the Wear device prompts you, confirm that you are allowing 
    **ADB Debugging**. In the Android Wear app, you should 
    see the status change to:

    ```
    Host: connected
    Target: connected
    ```

6. After you complete the above steps, running `adb devices` shows the
    status of both the phone and the Android Wear device:

    ```
    List of devices attached
    127.0.0.1:4444    device
    019ad61df0a69399  device
    ```

At this point, you can deploy your app to the Wear device.

<a name="screenshots"></a>

### Taking screenshots

You can take a screenshot of the Wear device by entering the following 
command: 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

Copy the screenshot to your computer by entering the following command:

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

Delete the screenshot on the device by entering the following command:

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```

### Uninstalling an app

You can uninstall an app from the wear device by entering the
following command:

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

For example, to remove the app with the package name `com.xamarin.weartest`,
enter the following command:

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

For more information about debugging Android Wear devices over
Bluetooth, see 
[Debugging over Bluetooth](https://developer.android.com/training/wearables/apps/bt-debugging.html).

## Debugging a Wear app with a companion phone app

Android Wear apps are packaged with a companion Android phone app for
distribution on Google Play (for more information, see
[Working with Packaging](~/android/wear/deploy-test/packaging.md)). 
However, you still develop the Wear app and its companion app
separately. When you release your app through the Google Play Store,
the Wear app will be packaged with the companion app and automatically
installed if possible.

To debug the Wear app with a companion app: 

1. Build and deploy the companion app to the phone.

2. Right-click the Wear project and set it as the default start
    project.

3. Deploy the Wear project to the wearable device.

4. Run and debug the Wear app on the device.

## Summary

This article explained how to configure an Android Wear device for Wear
debug from Visual Studio via Bluetooth, and how to debug a Wear app
with a companion phone app. It also provided common debugging tips for
debugging a Wear app via Bluetooth.
