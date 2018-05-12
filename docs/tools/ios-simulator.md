---
title: "Remoted iOS Simulator for Windows"
description: "The Remoted iOS Simulator for Windows allows you to test your apps on an iOS simulator displayed in Windows alongside Visual Studio 2017."
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: topgenorth
ms.author: toopge
ms.date: 05/11/2018
---

# Remoted iOS Simulator for Windows

The Remoted iOS Simulator for Windows allows you to test your apps on an
iOS simulator displayed in Windows alongside Visual Studio 2017.

[![](ios-simulator-images/hero-sml.png "iOS Simulator running on Windows")](ios-simulator-images/hero.png#lightbox)

## Getting started

The Remoted iOS Simulator for Windows is installed automatically as part
of Xamarin in Visual Studio 2017. To use it, follow these steps:

1. [Pair Visual 2017 to a Mac Build host](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. In Visual Studio 2017, start debugging an iOS or tvOS project. The 
Remoted iOS Simulator for Windows will appear on your Windows machine.

## Simulator window

The toolbar at the top of the simulator's window contains a number of 
useful buttons:

- **Home** – Simulates the home button on an iOS device
- **Lock** – Locks the simulator (swipe to unlock)
- **Screenshot** – Saves a screenshot of the simulator
- [**Settings**](#settings) – Displays keyboard, location, and other 
  settings
- [**Other options**](#other-options) – Brings up various simulator options 
  such as rotation and shake gestures

    [![](ios-simulator-images/maps-app-sml.png "iOS simulator maps example")](ios-simulator-images/maps-app.png#lightbox)

## Settings

Clicking the toolbar's gear icon opens the **Settings** window:

[![](ios-simulator-images/settings-sml.png "iOS simulator settings")](ios-simulator-images/settings.png#lightbox)

These settings allow you to enable the hardware keyboard, choose a
location that the device should report (static and moving locations are
both supported), enable Touch ID, and reset the content and settings for
the simulator.

## Other options

The toolbar's ellipsis button reveals other options such as rotation,
shake gestures, and rebooting. These same options can be viewed as a list
by right-clicking anywhere in the simulator's window:

[![](ios-simulator-images/more-sml.png "iOS simulator additional settings")](ios-simulator-images/more.png#lightbox)

## Touchscreen support

Most modern Windows computers have touch screens. Since the Remoted iOS
Simulator for Windows supports touch interactions, you can test your app
with the same pinch, swipe, and multi-finger touch gestures that you use
with physical iOS devices.

Similarly, the Remoted iOS Simulator for Windows treats Windows Stylus
input as Apple Pencil input.

## Disabling the Remoted iOS Simulator for Windows

To disable the Remoted iOS Simulator for Windows, navigate to 
**Tools > Options > Xamarin > iOS Settings** and uncheck 
**Remote Simulator to Windows**.

[![](ios-simulator-images/options-sml.png "checkbox to use simulator")](ios-simulator-images/options.png#lightbox)

With this option disabled, debugging opens the iOS Simulator on 
the connected Mac build host.
