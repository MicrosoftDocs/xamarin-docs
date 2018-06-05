---
title: "Inspector Installation and Requirements"
description: "This document describes how to install the Xamarin Inspector and discusses supported operating system, IDEs, and app platforms."
ms.prod: xamarin
ms.assetid: 81174493-02D3-4FF5-AD57-04F3288A7F94
author: topgenorth
ms.author: toopge
ms.date: 03/29/2017
---

# Inspector Installation and Requirements

## Download and Installation

# [Windows](#tab/windows)

1. Download and install [Xamarin Workbooks & Inspector for Windows](https://dl.xamarin.com/interactive/XamarinInteractive.msi).
2. [Inspect your own app!](~/tools/inspector/inspect.md)

# [macOS](#tab/macos)

1. Download and install [Xamarin Workbooks & Inspector for Mac](https://dl.xamarin.com/interactive/XamarinInteractive.pkg).
2. [Inspect your own app!](~/tools/inspector/inspect.md)

-----

## Requirements

### Supported Operating Systems

- **Mac** - OS X 10.11 or greater
- **Windows** - Windows 7 or greater (with Internet Explorer 11 or greater and
  .NET 4.6.1 or greater)

### Supported IDEs

- Xamarin Studio 6.2 or greater
- Visual Studio for Mac Preview 4 or greater
- Visual Studio 2015 with Xamarin 4.3.x or greater
- Visual Studio 2017 with Xamarin workload

Live app inspection is available for enterprise customers.

<a name="supported-platforms" />

### Supported App Platforms

|App Platform|IDE Support|Notes|
|--- |--- |--- |
|Mac (Unified)|Only supported on Mac|
|iOS (Unified)|Supported in XS and Visual Studio|Inspecting iOS apps from Windows requires the same version of Inspector to also be installed on the Mac build host.|
|Android|Supported in XS and Visual Studio|Must target Android >= 4.0.3, with **fastdev** enabled.<br />Must use Google, Visual Studio, or Xamarin Android emulators. Android 7 emulators may not allow inspection at this time.|
|WPF|Only supported in Visual Studio on Windows|

<a name="reporting-bugs" />

## Reporting Bugs

Bugs should be reported directly via Visual Studio:

- **Help > Send Feedback > Report a Problem**

Please include all of the following information:

### Platform Version Information

This information is vital.

Visual Studio For Mac

- **Visual Studio > About Visual Studio > Show Details > Copy Information**
- Paste into bug report

Xamarin Studio

- **Xamarin Studio > About Xamarin Studio > Show Details > Copy Information**
- Paste into bug report

Visual Studio

- **Help > About Visual Studio > Copy Info**
- Let us know your Operating System version and whether you are running 32-bit or 64-bit Windows.

### Log Files

Always attach both IDE and Inspector client log files.

Inspector client

- Mac: `~/Library/Logs/Xamarin/Inspector/Xamarin Inspector {date}.log`
- Windows: `%LOCALAPPDATA%\Xamarin\Inspector\logs\Xamarin Inspector {date}.log`

1.4.x also features the ability to select the log file in Finder (macOS) or
Explorer (Windows) directly from the main menu:

- **Help > Reveal Log File**

Visual Studio For Mac

- `~/Library/Logs/VisualStudio/7.0/Ide.log`

Xamarin Studio

- `~/Library/Logs/XamarinStudio-6.0/Ide.log`

Visual Studio

- `%LOCALAPPDATA%\Xamarin\Logs\{VS version}\Inspector {date}.log`
- The contents of the Visual Studio **Output** pane may also be informative.

### Project Settings

If you can attach the **.csproj** for the project you are trying to inspect,
it would be extremely helpful. This is easier than asking about individual settings.

Also please confirm that you are in Debug configuration.

### Selected Devices

For Android and iOS, it is vital that we know what device you are debugging on when
you want to inspect. We need to know:

- Name of device as shown in your IDE
- OS version of your device
- Android: Verify that you are using an x86 emulator
- Android: What emulator platform are you using? Google Emulator? Visual Studio Android Emulator? Xamarin Android Player?
- Does the app you are debugging properly appear and function in the device?
- Does the device have network connectivity (check via web browser)?

[client-bugs]: https://github.com/Microsoft/workbooks/issues/new

## Uninstall

### Windows

Depending on how you acquired Workbooks & Inspector, you may have to perform
two uninstallation procedures. Please check both of these to completely
uninstall the software.

#### Visual Studio Installer

If you have Visual Studio 2017, open **Visual Studio Installer**, and look in
**Individual Components** for **Xamarin Workbooks**. If it is checked, uncheck it
and then click "Modify" to uninstall.

#### System Uninstall

If you installed Workbooks & Inspector yourself with a downloaded installer,
it will need to be uninstalled via the **Apps & features**
system settings page on Windows 10 or via **Add/Remove Programs** in the
Control Panel on older versions of Windows.

> **Start > Settings > System > Apps & features**

![](install-images/windows-remove.png "Xamarin Workbooks and Inspector as listed in 'Apps & features'")

**You should still follow the procedure for the Visual Studio Installer to make
sure Workbooks & Inspector does not get reinstalled without your knowledge.**

### macOS

Starting with [1.2.2](https://developer.xamarin.com/releases/interactive/interactive-1.2/),
Xamarin Workbooks & Inspector can be uninstalled from a terminal by running:

```bash
sudo /Library/Frameworks/Xamarin.Interactive.framework/Versions/Current/uninstall
```

The uninstaller will detail the files and directories it will remove and
ask for confirmation before proceeding.

Pass the `-help` argument to the `uninstall` script for more advanced
scenarios.

For older versions, you will need to manually remove the following:

1. Delete the Workbooks app at `"/Applications/Xamarin Workbooks.app"`
2. Delete the Inspector app at `"Applications/Xamarin Inspector.app"`
3. Delete the add-ins: `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Interactive"` and `"~/Library/Application Support/XamarinStudio-6.0/LocalInstall/Addins/Xamarin.Inspector"`
4. Delete Inspector and supporting files here: `/Library/Frameworks/Xamarin.Interactive.framework` and `/Library/Frameworks/Xamarin.Inspector.framework`
