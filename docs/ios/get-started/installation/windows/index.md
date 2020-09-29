---
title: "Installing Xamarin.iOS on Windows"
description: "This document describes how to set up a Windows machine, set up a Mac build host, and pair Windows to the Mac for Xamarin.iOS development."
zone_pivot_groups: platform-win
ms.prod: xamarin
ms.assetid: abf85d3e-a365-44a2-b1a4-6c572c7f76dd
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/16/2018
---

# Installing Xamarin.iOS on Windows

_This article describes how to set up a Windows machine and a Mac build
host for Xamarin.iOS development._

::: zone pivot="windows"

## Overview

To build Xamarin.iOS apps with Visual Studio 2019 on Windows, you will need:

- A Windows machine with Visual Studio 2019 installed. This can be a physical
  or a virtual machine.

  - [Windows system requirements](~/cross-platform/get-started/requirements.md#windows-requirements)

- A network-accessible Mac set up with Apple's build tools
  and Xamarin.iOS. Visual Studio 2019 accesses this machine over a network
  connection to use Apple's build tools, which are required for compiling
  native iOS applications.

  - [Mac system requirements](~/cross-platform/get-started/requirements.md#macos-requirements)

  > [!TIP]
  > Don't have access to a Mac?
  >
  > If you do not have access to a Mac, you can use [MacinCloud](https://www.macincloud.com/pages/visual-studio-mac.html) or [MacStadium](https://www.macstadium.com/); both services provide cloud-based Mac hardware that you can use to build Xamarin.iOS projects.

## Setup

To get set up for Xamarin.iOS development in Visual Studio 2019, follow
these steps:

1. Set up Windows (Install Visual Studio 2019)

    Xamarin.iOS works with Visual Studio 2019 Community, Professional,
    and Enterprise editions, on a standalone or virtual machine.

    - [Install Visual Studio 2019](~/get-started/installation/windows.md).

2. Set up Mac (Install Xcode and Visual Studio for Mac)

    To build, debug, and sign iOS applications for distribution, Visual
    Studio 2017 must have network access to a Mac build host configured
    with both Apple's developer tools (Xcode) and Xamarin.iOS.

    - [Download and install Xcode from the Mac App
      Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).
    - [Install Visual Studio for
      Mac](/visualstudio/mac/installation), which
      also installs Xamarin.iOS.

    > [!NOTE]
    > If you would prefer not to install Visual Studio for Mac, Visual Studio 2019 can
    > [automatically configure](/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning),
    > the Mac build host with the software necessary to build Xamarin.iOS applications.
    > For more information, see [Automatic Mac provisioning](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning).

3. Pair to Mac (Connect Visual Studio 2019 to the Mac)

    For Visual Studio 2019 to use the iOS build tools on the Mac, the two
    machines must connect over a network.

    - [Read the Pair to Mac guide](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

::: zone-end
::: zone pivot="win-vs2017"

## Overview

To build Xamarin.iOS apps with Visual Studio 2017 on Windows, you will need:

- A Windows machine with Visual Studio 2017 installed. This can be a physical
  or a virtual machine.
  - [Windows system requirements](~/cross-platform/get-started/requirements.md#windows-requirements)

- A network-accessible Mac set up with Apple's build tools
  and Xamarin.iOS. Visual Studio 2017 accesses this machine over a network
  connection to use Apple's build tools, which are required for compiling
  native iOS applications.
  - [Mac system requirements](~/cross-platform/get-started/requirements.md#macos-requirements)

## Setup

To get set up for Xamarin.iOS development in Visual Studio 2017, follow
these steps:

1. Set up Windows (Install Visual Studio 2017)

    Xamarin.iOS works with Visual Studio 2017 Community, Professional,
    and Enterprise editions, on a standalone or virtual machine.

    - [Install Visual Studio 2017](~/get-started/installation/windows.md).

2. Set up Mac (Install Xcode and Visual Studio for Mac)

    To build, debug, and sign iOS applications for distribution, Visual
    Studio 2017 must have network access to a Mac build host configured
    with both Apple's developer tools (Xcode) and Xamarin.iOS.

    - [Download and install Xcode from the Mac App
      Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).
    - [Install Visual Studio for
      Mac](/visualstudio/mac/installation), which
      also installs Xamarin.iOS.

    > [!NOTE]
    > If you would prefer not to install Visual Studio for Mac, starting with
    > [Visual Studio 2017 version 15.6](/visualstudio/releasenotes/vs2017-relnotes#automatic-macos-provisioning),
    > Visual Studio 2017 can automatically configure the Mac build host with
    > the software necessary to build Xamarin.iOS applications. For more
    > information, see [Automatic Mac provisioning](~/ios/get-started/installation/windows/connecting-to-mac/index.md#automatic-mac-provisioning).

3. Pair to Mac (Connect Visual Studio 2017 to the Mac)

    For Visual Studio 2017 to use the iOS build tools on the Mac, the two
    machines must connect over a network.

    - [Read the Pair to Mac guide](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

::: zone-end

## Summary

This article described how to set up a Windows machine and its associated
Mac build host for Xamarin.iOS development.

## Next steps

- [Introduction to Xamarin.iOS for Visual Studio](introduction-to-xamarin-ios-for-visual-studio.md)
- [Configuring Visual Studio for iOS development](config-options.md)
- [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md)