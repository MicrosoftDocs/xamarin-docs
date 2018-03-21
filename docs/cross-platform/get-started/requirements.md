---
title: "System Requirements"
description: "Pre-requisites for using Xamarin"
ms.topic: article
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 08/28/2017
---

# System Requirements

_Pre-requisites for using Xamarin_

Xamarin products rely upon the platform SDKs from Apple and Google to
target iOS or Android, so our system requirements match theirs. This page
outlines system compatibility for the Xamarin platform and recommended
development environment and SDK versions.

- [Development Environments](#devenv)
- [macOS Requirements](#mac)
- [Windows Requirements](#windows)

Visit the [installation instructions](#install) for more information
on obtaining the software and required SDKs.

<a name="devenv" />

## Development Environments

This table shows which platforms can be built with different
development tool & operating system combinations:

[!include[](~/cross-platform/includes/development-environment.md)]


> [!NOTE]
> To develop for iOS on Windows computers there must be a
> [Mac computer accessible on the network](~/ios/get-started/installation/windows/connecting-to-mac/index.md),
> for remote compilation and debugging. This also works if you have Visual Studio
> running inside a Windows VM on a Mac computer.

<a name="mac" />

## macOS Requirements

Using a Mac computer for Xamarin development requires the following software/SDK versions. Check
your operating system version and follow the instructions for the [Xamarin installer](#install).

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Xcode can be installed (and updated) on
>[developer.apple.com](https://developer.apple.com/xcode/download/) or via the Mac App Store.

### Testing & Debugging on macOS

Xamarin mobile applications can be deployed to physical devices via USB
for testing and debugging
(Xamarin.Mac apps can be tested directly on the development computer;
Apple Watch apps are deployed first to the paired iPhone).

[!include[](~/cross-platform/includes/macos-testing.md)]


<a name="windows" />

## Windows Requirements

Using a Windows computer for Xamarin development requires the following software/SDK versions.
Check your operating system version (and confirm that you are not using an *Express* version of
Visual Studio - if so, consider updating to a *Community* edition).
Visual Studio 2015 and 2017 installers include an option to install Xamarin automatically.

[!include[](~/cross-platform/includes/windows-requirements.md)]


> [!NOTE]
>
>* Xamarin for Visual Studio supports any Visual Studio 2015 or 2017 (Community, Professional, and Enterprise).
>
>* To develop Xamarin.Forms apps for the Universal Windows Platform (UWP) requires
>  Windows 10 with Visual Studio 2015 or 2017.


### Testing & Debugging on Windows

Xamarin mobile applications can be deployed to physical devices via USB
for testing and debugging (iOS devices must be connected to the Mac computer, not the computer
running Visual Studio).

[!include[](~/cross-platform/includes/windows-testing.md)]


> [!NOTE]
>
>* [Windows Phone 8.1 emulator download](https://www.microsoft.com/en-us/download/details.aspx?id=43719).
>* The Windows Phone 10 emulator is included with the Visual Studio 2015 UWP SDK.

<a name="install" />

## Installation Instructions

The latest Xamarin release for macOS can be downloaded from
[xamarin.com/download](http://xamarin.com/download). For Windows,
follow the [Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/install-visual-studio)
installation instructions.

A complete list of our current product versions is available on the
[current releases page](http://developer.xamarin.com/releases/current/). This
page also outlines the individual product versions (and links to the release notes)
for our beta and alpha channels.

Specific [installation](~/cross-platform/get-started/installation/index.md) instructions for each platform are available here:

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

There's also additional information about
[Xamarin.Forms requirements & supported platforms](~/xamarin-forms/get-started/installation.md).


## Related Links

- [Download Xamarin](https://xamarin.com/download/)
- [Current Releases](https://developer.xamarin.com/releases/current/)
