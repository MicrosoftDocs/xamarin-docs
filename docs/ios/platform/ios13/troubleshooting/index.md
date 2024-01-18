---
title: "Xamarin and iOS 13 troubleshooting"
description: "This section contains troubleshooting tips for Xamarin functionality related to iOS 13."
ms.service: xamarin
ms.assetid: 00DE8C33-1407-45C0-A0C7-32AF1E490034
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/12/2019
no-loc: [Objective-C]
---
# Troubleshooting tips for iOS 13 and Xamarin.iOS

## Updating to Xcode 11 stops the simulator from launching

After updating to **Xcode 11 beta 1** every time a simulator is launched the following exception is thrown, and the simulator does not start. This happens with all simulators.

### Exception

`Foundation.ObjCException: NSInvalidArgumentException: -[SimDevice registerNotificationHandler:]: unrecognized selector sent to instance 0x7ffbf5d1e110`

### Workaround

Until there is a [fix](https://github.com/xamarin/xamarin-macios/issues/6216), the following steps can be followed to re-install the old simulator framework to allow developers to continue to work:

> [!NOTE]
> These steps assume you have two Xcode applications:
>
> - **Xcode11-beta1.app** – The beta version which does not work with simulators and Visual Studio for Mac.
> - **Xcode102.app** – A stable version of Xcode 10. Yours might also be called **Xcode.app**.
>
> Change the command line examples below as appropriate for your configuration.

1. Ensure that you have Xcode 11 selected via xcode-select:

   `sudo xcode-select -s /Applications/Xcode11-beta1.app/Contents/Developer/`

2. Run, if needed, the setup tools for the first time.

    `/Applications/Xcode11-beta1.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

3. Remove the following framework:

    `sudo rm -Rf  /Library/Developer/PrivateFrameworks/CoreSimulator.framework/Versions/*`

4. Switch back to the old Xcode version

   `sudo xcode-select -s /Applications/Xcode102.app/Contents/Developer/`

5. Re-run the first launch tool for the _OLD_ Xcode version you just selected

   `/Applications/Xcode102.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

After following these steps, you should be able to work with the iOS simulator again.
