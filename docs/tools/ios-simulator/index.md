---
title: "Remoted iOS Simulator for Windows"
description: "The Remoted iOS Simulator for Windows allows you to test your apps on an iOS simulator displayed in Windows alongside Visual Studio 2019."
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: davidortinau
ms.author: daortin
ms.date: 04/26/2019
---

# Remoted iOS Simulator for Windows

The Remoted iOS Simulator for Windows allows you to test your apps on an
iOS simulator displayed in Windows alongside Visual Studio 2019 and Visual Studio 2017.

[![iOS simulator running on Windows](images/hero-sml.png "iOS simulator running on Windows")](images/hero.png#lightbox)

## Getting started

The Remoted iOS Simulator for Windows is installed automatically as part
of Xamarin in Visual Studio 2019 and Visual Studio 2017. To use it, follow these steps:

1. [Pair Visual Studio 2019 to a Mac Build host](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. In Visual Studio, start debugging an iOS or tvOS project. The
Remoted iOS Simulator for Windows will appear on your Windows machine.

Watch [this video](deploy.md) for a step-by-step guide.

## Simulator window

The toolbar at the top of the simulator's window contains a number of useful buttons:

- **Home** – Simulates the home button on an iOS device.
- **Lock** – Locks the simulator (swipe to unlock).
- **Screenshot** – Saves a screenshot of the simulator (stored in **Pictures\Xamarin\iOS Simulator\\**).
- [**Settings**](#settings) – Displays keyboard, location, and other settings.
- [**Other options**](#other-options) – Brings up various simulator options such as rotation, shake gestures, and Touch ID.

    [![iOS simulator maps example](images/maps-app-sml.png "iOS simulator maps example")](images/maps-app.png#lightbox)

## Settings

Clicking the toolbar's gear icon opens the **Settings** window:

[![iOS simulator settings](images/settings-sml.png "iOS simulator settings")](images/settings.png#lightbox)

These settings allow you to enable the hardware keyboard, choose a
location that the device should report (static and moving locations are
both supported), enable Touch ID, and reset the content and settings for
the simulator.

## Other options

The toolbar's ellipsis button reveals other options such as rotation,
shake gestures, and rebooting. These same options can be viewed as a list
by right-clicking anywhere in the simulator's window:

[![iOS simulator additional settings](images/more-sml.png "iOS simulator additional settings")](images/more.png#lightbox)

## Touchscreen support

Most modern Windows computers have touch screens. Since the Remoted iOS
Simulator for Windows supports touch interactions, you can test your app
with the same pinch, swipe, and multi-finger touch gestures that you use
with physical iOS devices.

Similarly, the Remoted iOS Simulator for Windows treats Windows Stylus
input as Apple Pencil input.

## Sound handling

Sounds played by the simulator will come from the host Mac's speakers.
iOS sounds are not heard on the Windows computer.

## Disabling the Remoted iOS Simulator for Windows

To disable the Remoted iOS Simulator for Windows, navigate to
**Tools > Options > Xamarin > iOS Settings** and uncheck
**Remote Simulator to Windows**.

[![checkbox to use simulator](images/options-sml.png "checkbox to use simulator")](images/options.png#lightbox)

With this option disabled, debugging opens the iOS Simulator on the connected Mac build host.

## Troubleshooting

If you experience issues with the Remoted iOS Simulator, you can view the logs in these locations:

- **Mac** – `~/Library/Logs/Xamarin/Simulator.Server`
- **Windows** – `%LOCALAPPDATA%\Xamarin\Logs\Xamarin.Simulator`

If you [report a problem in Visual Studio](/visualstudio/ide/how-to-report-a-problem-with-visual-studio), attaching these logs might be helpful (there are options to keep uploads private).