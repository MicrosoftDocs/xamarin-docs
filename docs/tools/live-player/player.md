---
title: "Xamarin Live Player App"
description: "This document describes the Xamarin Live Player app, which can be used to preview code changes live on device. It discusses setup, samples, logs, settings, managing devices, and more."
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: davidortinau
ms.author: daortin
ms.date: 06/13/2019
---

# Xamarin Live Player App

![Preview feature](~/media/shared/preview.png)

> [!WARNING]
> The Xamarin Live Player Preview has ended. The app is no longer available. The instructions below are provided for customers continuing to use the preview with Visual Studio 2017.

> [!TIP]
> You can use the [XAML Previewer](~/xamarin-forms/xaml/xaml-previewer/index.md) in Visual Studio 2019 or 
> Visual Studio for Mac to view your screen designs as you edit them.

On startup, the Xamarin Live Player app looks like this:

![Live Player Android app screenshot](player-images/app-android-sml.png)

When you press **Pair to Visual Studio**, use the camera to scan the
barcode showing on your computer:

![Screenshot of the Android barcode scanner](player-images/scan-android-sml.png)

If the connection is successful, the code should run on
the device almost immediately (such as the [Calculator sample](https://github.com/xamarin/mobile-samples/tree/master/LivePlayer/BasicCalculator):

![Sample calculator app running on device](player-images/basic-calculator-sml.png)

## Options

Press the information button **(i)** on the bottom of the app to reveal the **Options** menu:

[![Screenshot of the options menu](player-images/options-sml.png)](player-images/options.png#lightbox)

### Logs

View logs to diagnose problems.

### Settings

- Toggle display of compile and runtime errors.
- Version information.
- Send feedback.

[![Screenshot of the settings](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## Managing Devices

To connect a device for the first time, follow the instructions in [Requirements & Setup](~/tools/live-player/install.md). You can pair multiple devices and manage them via the IDE.

# [Visual Studio 2017](#tab/windows)

In Visual Studio, choose **Tools > Xamarin Live Player > Manage Devices...**

![Manage devices window](player-images/manage-tools-menu-vs.png)

This window lets you do the following:

- Pair a new device by scanning the code
- Alternatively pair a device by typing the code displayed on its screen
- Remove existing devices from the list

You can also access this window from the device list.

# [Visual Studio for Mac](#tab/macos)

In Visual Studio for Mac, choose **Tools > (Xamarin Live Player) Manage Devices...**

![Screenshot shows Manage Devices selected from the Tools window.](player-images/manage-tools-menu.png)

This window lets you do the following:

- Pair a new device by scanning the code
- Alternatively pair a device by typing the code displayed on its screen
- Remove existing devices from the list

![Screenshot shows the Xamarin Live Player window with the option to preview your app.](player-images/manage.png)

You can also access this window from the device list:

![Choose Xamarin Live Player Devices from the device list](player-images/manage-device-menu.png)

-----

If you experience any issues see [limitations and troubleshooting](~/tools/live-player/troubleshooting.md).

## Related Links

- [Troubleshooting](~/tools/live-player/troubleshooting.md)
