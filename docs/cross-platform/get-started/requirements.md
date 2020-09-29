---
title: "System Requirements"
description: "This document lists the system requirements for building apps with Xamarin on both Mac and Windows computers. It also links to installation instructions."
ms.prod: xamarin
ms.assetid: dd344d57-18e2-42a5-8c15-3f5be4123c72
author: davidortinau
ms.author: daortin
ms.date: 10/16/2019
---
# System requirements

Xamarin products rely upon the platform SDKs from Apple and Google to
target iOS or Android, so our system requirements match theirs. This page
outlines system compatibility for the Xamarin platform and recommended
development environment and SDK versions.

Take a look at the [installation instructions](#installation-instructions)
for more information on obtaining the software and required SDKs.

## Development environments

This table shows which platforms can be built with different
development tool & operating system combinations:

[!include[](~/cross-platform/includes/development-environment.md)]

> [!NOTE]
> To develop for iOS on Windows computers there must be a
> [Mac computer accessible on the network](~/ios/get-started/installation/windows/connecting-to-mac/index.md),
> for remote compilation and debugging. This also works if you have Visual Studio
> running inside a Windows VM on a Mac computer.

## macOS requirements

Using a Mac computer for Xamarin development requires the following software/SDK versions. Check
your operating system version and follow the instructions for the [Xamarin installer](#installation-instructions).

[!include[](~/cross-platform/includes/macos-requirements.md)]

> [!NOTE]
> Xcode can be installed (and updated) on
>[developer.apple.com](https://developer.apple.com/xcode/download/) or via the Mac App Store.

### Testing & debugging on macOS

- Xamarin mobile applications can be deployed to physical devices via USB
for testing and debugging (Apple Watch apps are deployed first to the
paired iPhone).
- Xamarin.Mac apps can be tested directly on the development computer.

[!include[](~/cross-platform/includes/macos-testing.md)]

> [!WARNING]
> Xamarin.Mac 4.8 only supports macOS 10.9 (Mavericks) or higher.
> Previous versions of Xamarin.Mac supported macOS 10.7 or higher, but
> these older macOS versions lack sufficient TLS infrastructure to support
> TLS 1.2. To target macOS 10.7 or macOS 10.8, use Xamarin.Mac 4.6 or
> earlier.

## Windows requirements

Using a Windows computer for Xamarin development requires the following software/SDK versions.
Check your operating system version (and confirm that you are not using an *Express* version of
Visual Studio - if so, consider updating to a *Community* edition).
The Visual Studio 2019 and Visual Studio 2017 installer includes an option to install Xamarin automatically (the **Mobile development with .NET** workload).

[!include[](~/cross-platform/includes/windows-requirements.md)]

> [!NOTE]
>
> - Xamarin for Visual Studio supports Visual Studio 2019 or Visual Studio 2017 (Community, Professional, and Enterprise).
> - To use the latest Android and iOS SDKs requires the latest version of Visual Studio. For specific version requirements, refer to the  [Xamarin.Android release notes](/xamarin/android/release-notes/) and [Xamarin.iOS release notes](/xamarin/ios/release-notes/).
> - To develop Xamarin.Forms apps for the Universal Windows Platform (UWP) requires
>   Windows 10 with Visual Studio 2017. Visual Studio 2019 is recommended.

### Testing & debugging on Windows

Xamarin mobile applications can be deployed to physical devices via USB
or wirelessly for testing and debugging (iOS devices must be connected to
the Mac computer, not the computer running Visual Studio).

[!include[](~/cross-platform/includes/windows-testing.md)]

## Installation instructions

The latest Xamarin release for macOS can be downloaded with [Visual Studio for Mac](/visualstudio/mac/installation). For Windows,
follow the [Visual Studio installation instructions](/visualstudio/install/install-visual-studio).

A complete list of our current product releases is available on the
[what's new page](~/whats-new/index.yml). This
page also links to the release notes.

Specific [installation](~/get-started/installation/index.md) instructions for each platform are available here:

- [Xamarin.iOS](~/ios/get-started/installation/index.md)
- [Xamarin.Android](~/android/get-started/installation/index.md)
- [Xamarin.Mac](~/mac/get-started/installation.md)

There's also additional information about
[Xamarin.Forms supported platforms](~/get-started/supported-platforms.md).

## Related links

- [Download Xamarin](https://visualstudio.microsoft.com/xamarin/)
- [Xamarin.Forms release notes](/xamarin/xamarin-forms/release-notes/)
- [Xamarin.Android release notes](/xamarin/android/release-notes/)
- [Xamarin.iOS release notes](/xamarin/ios/release-notes/)