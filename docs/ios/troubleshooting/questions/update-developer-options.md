---
title: "How can I reenable developer options after updating iOS?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F38BD21E-0C21-43FF-80A6-BB4A88DB88A5
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
---

# How can I reenable developer options after updating iOS?

An iOS bug may cause the developer options to disappear after updating iOS versions, this has been observed when switching to iOS 8.x. These options can be reenabled with the following steps:

1. Connect the iOS device to your Mac development machine.
2. Start Xcode.
3. Open **Window > Devices** & select your device.
4. Wait for Xcode to finish copying symbols and then check the device's Settings.app for the Developer settings.
5. Reboot the device.
