---
title: "Where's my Mac?"
description: "Instructions for configuring your Mac for Visual Studio to build Xamarin.iOS projects"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4f792690-b3b1-4d56-a5e2-363b4f246059
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
---

# Where's my Mac?

_Instructions for configuring your Mac for Visual Studio to build Xamarin.iOS projects_

The current **stable** version of Xamarin for Visual Studio interacts with your
Mac differently to previous versions[^](#earlier-versions).

To use a Mac as a Xamarin Mac Agent, you must enable **Remote Login** on your Mac.

1. Open *Spotlight* (`Cmd-Space`) and search for **Remote Login** then
and select the **Sharing** result. This will open the **System Preferences** at the **Sharing** panel.

  ![](visual-studio-ssh-images/spotlight.png "Spotlight search for remote login")

2. Tick the **Remote Login** option in the **Service** list on the left in order
to allow Xamarin for Visual Studio to connect to the Mac.

  ![](visual-studio-ssh-images/sharing.png "Tick the Remote Login option in the Service list")

3. Make sure that **Remote Login** is set to allow access for **All users**,
or that your Mac username or group is included in the list of allowed users
in the list on the right.

Your Mac should now be discoverable by Visual Studio if it's on the same network.
When Visual Studio attempts to connect to the Mac, you'll be prompted to login
from Visual Studio using your Mac user credentials.

## Where can I find more information?

There are a number of guides available to help you set up and understand the new agent, which are listed below:

- [Connecting to your Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
- [Troubleshooting](~/ios/get-started/installation/windows/connecting-to-mac/troubleshooting.md)
- [Xamarin University - Xamarin Mac Agent](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)

<a name="earlier-versions" />

## Migrating from previous versions

If you used earlier versions Xamarin for Visual Studio you would be familiar
with the **Xamarin.iOS Build Host** application which needed to be started
on your Mac, and then *paired* via a PIN number with your Visual Studio instance.

The build host application is no longer required with this version of
Xamarin for Visual Studio.
