---
title: "Troubleshooting"
description: "Known issues with the Xamarin Live Player, and how to fix them."
ms.topic: article
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
---

# Troubleshooting

![Preview feature](~/media/shared/preview.png)

This article explains some common problems and provides steps to correct them.


## Mobile device does not connect after scanning barcode (or entering code)

Occurs when the mobile device running Xamarin Live Player is not on the
same network as the computer running the IDE. Check out the following:

- Confirm that both the device and computer are on the same WiFi network.
  - If the computer is also connected to a wired network, try unplugging the wired connection.
- The network may be tightly secured (such as some corporate networks), blocking the ports needed by Xamarin Live Player.
- Close the Xamarin Live Player app and restart it.


## "Error while trying to deploy" message in IDE

**"IOException: unable to read data from the transport connection: Operation on non-blocking socket would block"**

This error is often experienced when the mobile device running Xamarin Live Player is not on the
same network as the computer running the IDE; this often happens when connecting to a device that was previously
paired successfully.

* Check that both the device and computer are on the same WiFi network.
* The network may be tightly secured (such as some corporate networks), blocking the ports needed by Xamarin Live Player. The following ports are required for the Xamarin Live Player:
  * 37847 – Internal network access 
  * 8090 – External network access

## "Type or namespace cannot be found" message in IDE

Check that you have selected a **Startup Project** that matches your device type (iOS or Android)
and that the configuration matches that device type (eg. **Debug|iPhone Simulator** for iOS).

## "Constructor on type 'InterpretedXamarin.Forms.Button' not found" message in Player

Some system classes cannot be overridden, for example:

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## "MainActivity.cs: 'Resource.Layout' does not contain a definition for 'Main'"

This error occurs for Android projects with user interfaces defined in AXML files.
AXML files are not currently supported in Xamarin Live Player.

### Android Toolbar and Tabs render incorrectly using Xamarin.Forms

Xamarin.Forms Android projects must use "Toolbar.axml" and "Tabbar.axml"
for the names of the relevant layout files. The default template
uses these names; renaming them will cause rendering issues.


Please report any additional issues on [bugzilla](https://aka.ms/live-player-report-issue).


## Related Links

- [Limitations](~/tools/live-player/limitations.md)
- [Setup](~/tools/live-player/install.md)
