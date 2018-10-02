---
title: "Troubleshooting Xamarin Live Player"
description: "This document describes known issues with the Xamarin Live Player and potential fixes. It discusses connection issues, configuration issues, and more."
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
---

# Troubleshooting Xamarin Live Player

![Preview feature](~/media/shared/preview.png)

This article explains some common problems and provides steps to correct them.

## Mobile device does not connect after scanning barcode (or entering code)

Occurs when the mobile device running Xamarin Live Player is not on the
same network as the computer running the IDE. Check out the following:

- Confirm that both the device and computer are on the same Wi-Fi network.
  - If the computer is also connected to a wired network, try unplugging the wired connection.
- The network may be tightly secured (such as some corporate networks), blocking the ports needed by Xamarin Live Player.
- Close the Xamarin Live Player app and restart it.

## "Error while trying to deploy" message in IDE

**"IOException: unable to read data from the transport connection: Operation on non-blocking socket would block"**

This error is often experienced when the mobile device running Xamarin Live Player is not on the
same network as the computer running Visual Studio; this often happens when connecting to a device that was previously
paired successfully.

* Check that both the device and computer are on the same Wi-Fi network.
* The network may be tightly secured (such as some corporate networks), blocking the ports needed by Xamarin Live Player. The following ports are required for the Xamarin Live Player:
  * 37847 – Internal network access 
  * 8090 – External network access

## Manually Configure Device

If you can not connect to your device over Wi-Fi you can attempt to manually configure your device via the configuration file, with the following steps:

**Step 1: Open configuration file**

Head to your application data folder:

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

In this folder you will find **PlayerDeviceList.xml** if it does not exist you will need to create one.

**Step 2: Get IP address**

In the Xamarin Live Player app, go to **About > Connection Test > Start Connection Test**.

Take note of the IP Address, you will need the IP address listed when you configure your device.

**Step 3: Get pairing code**

Inside of the Xamarin Live Player tap **Pair** or **Pair Again**, then press **Enter Manually**. A numeric code will be displayed, which you will need to update the configuration file.

**Step 4: Generate GUID**

Go to: https://www.guidgenerator.com/online-guid-generator.aspx and generate a new guid and make sure Upper Case is on.

**Step 5: Configure device**

Open up the **PlayerDeviceList.xml** up in an editor such as Visual Studio or Visual Studio Code. You need to configure your device manually in this file. By default, the file should contain the following empty `Devices` XML element:

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**Add Android Device:**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**Close and re-open Visual Studio.** Your device should show up in the list.

## "Type or namespace cannot be found" message in IDE

Check that you have selected a **Startup Project** that matches your device type (eg. Android)
and that the configuration matches that device type (eg. **Debug** for Android).

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
