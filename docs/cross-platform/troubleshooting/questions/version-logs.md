---
title: "Where can I find my version information and logs?"
description: "This document describes where to look to find Xamarin version information and logs. This information is useful when diagnosing problems, submitting bugs, or getting support."
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
---

# Where can I find my version information and logs?

## Outline

- [Version information](#version-information)
  - Windows version information
  - Mac version information
  - Android SDK Tools, Platform-tools, Build-tools
- [IDE and installer logs](#ide-and-installer-logs)
  - [Windows logs](#windows-logs)
    - Xamarin Studio
    - Xamarin for Visual Studio
    - Xamarin Universal installer
    - Individual `.msi` installers, verbose logs
    - Visual Studio startup, verbose logs
  - [Mac logs](#mac-logs)
    - Build host
  - Visual Studio for Mac
    - Xamarin Studio
    - Xamarin installer
- [Verbose build output](#verbose-build-output-logs)
- [Debug logs for Xamarin.Android and Xamarin.iOS apps](#debug-logs-for-xamarin-apps)
  - Android `adb` logcat logs
  - iOS simulator logs (on Mac)
  - iOS device logs (on Mac)

## <a id="version-information" name="version-information" />Version information

It's usually best to send back all of the information from the **Copy Information** buttons. Otherwise we will often need to request additional information. For example, operating system versions, Xcode version, installed Android API levels, and .NET version can all be important when helping to troubleshoot a problem.

### <a id="windows-version-information" name="windows-version-information" />Windows version information

#### Xamarin Studio

**Help > About > Show Details > Copy Information [button]**

#### Visual Studio

**Help > About Microsoft Visual Studio > Copy Info [button]**

### <a id="mac-version-information" name="mac-version-information" />Mac version information

#### Visual Studio for Mac

**Visual Studio > About Visual Studio > Show Details > Copy Information [button]**

### <a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK Tools, Platform-tools, Build-tools

Open the Android SDK Manager, and take a screenshot of the top **Tools** section.

#### Visual Studio for Mac

**Tools > Open Android SDK Manager**

#### Visual Studio

**Tools > Android > Open Android SDK Manager...**

## <a id="ide-and-installer-logs" name="ide-and-installer-logs" />IDE and installer logs

For each log location, be sure to zip up and attach the entire log folder.

### <a id="windows-logs" name="windows-logs" />Windows logs

#### <a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" /> Visual Studio Tools for Xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a id="vs-2017" name="vs-2017" /> Visual Studio 2017

[How to get the Visual Studio installation logs](/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a id="vs-2015" name="vs-2015" /> Visual Studio 2015

#### <a id="windows-universal-installer" name="windows-universal-installer" /> Xamarin "Universal" installer

`%LOCALAPPDATA%\Xamarin\Universal`

These are the logs from the `XamarinInstaller.exe` installer.

#### <a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />Individual `.msi` installers, verbose logs

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Reference: [Command-Line Options](/windows/win32/msi/command-line-options)

#### <a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Visual Studio startup, verbose logs

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Reference: [/Log (devenv.exe)](/visualstudio/ide/reference/log-devenv-exe)

### <a id="mac-logs" name="mac-logs" />Mac logs

You can select the **Go > Go to Folder** menu item in Finder, and then copy and paste any of these paths into the dialog.

#### <a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio for Mac

`~/Library/Logs/VisualStudio/7.0` (this number may change depending on the version you are using)

This folder can also be opened via "Help -> Open Log Directory".

#### <a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0` (this number may change depending on the version you are using)

This folder can also be opened via "Help -> Open Log Directory".

#### <a id="mac-universal-installer" name="mac-universal-installer" />Xamarin "Universal" installer

`~/Library/Logs/XamarinInstaller/Universal`

These are the logs from the `XamarinInstaller.dmg` installer.

#### <a id="mac-build-host" name="mac-build-host" />Xamarin Build Host

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a id="verbose-build-output-logs" name="verbose-build-output-logs" />Verbose build output

1. Enable [diagnostic MSBuild output](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).

2. For iOS apps, also enable **verbose mtouch output** by adding `-v -v -v -v` under **Project Properties > iOS Build > General (tab) > Additional Options > Additional mtouch arguments**.

3. Clean and rebuild the project.

4. Copy and paste the build output from the IDE into a text file.
     - Visual Studio (Windows): **View > Output > Show output from: Build**
     - Visual Studio for Mac: **View > Pads > Errors > Build Output (tab)**

## <a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Debug logs for Xamarin.Android and Xamarin.iOS apps

### Visual Studio for Mac

**View > Pads > Application Output**

(Note that this menu item will only appear after the app has been launched.)

### Visual Studio

**View > Output > Show output from: Debug**

### <a id="adb-logcat" name="adb-logcat" />Android [`adb`](https://developer.android.com/tools/help/adb.html) logcat logs

After running the `adb` command, attach back the **android_logcat.txt** file from your Desktop. These instructions assume you have only one device attached.

See also the [Android Debug Log](~/android/deploy-test/debugging/android-debug-log.md) page.

#### Visual Studio

1. **Tools > Android > Start Android Adb Command Prompt**
2. Clean the log: `adb logcat -c`
3. Reproduce the issue.
4. Output the log: `adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### Visual Studio for Mac

1. **Tools > Open Android SDK Command Prompt**
2. Clean the log: `adb logcat -c`
3. Reproduce the issue.
4. Output the log: `adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a id="ios-simulator-logs" name="ios-simulator-logs" />iOS simulator logs (on Mac)

- To access the system log, select **Debug > Open System Log...** in the iOS Simulator app.

- To view crash reports from the simulator, open Console.app and navigate to `~/Library/Logs > DiagnosticReports`.

### <a id="ios-device-logs" name="ios-device-logs" />iOS device logs (on Mac)

#### Visual Studio for Mac

**View > Pads > iOS Device Log**

#### Xcode

**Window > Devices > ${DeviceName}**

Crash reports are available under the **View Device Logs** button. The system log for the device appears at the bottom of the window under the disclosure arrow.

#### Xcode 5

**Window > Organizer > Devices (tab) > ${DeviceName}**