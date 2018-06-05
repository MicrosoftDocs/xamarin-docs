---
title: "Installing Xamarin in Visual Studio 2017"
description: "This document describes how to install Xamarin in Visual Studio 2017. It discusses requirements, the installation process, and verifying the installation."
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
---

# Installing Xamarin in Visual Studio 2017

<a name="requirements" />

## Requirements

The following are required for installing Xamarin in Visual Studio 2017:

1. Windows 7 or higher.

2. Visual Studio 2017 (Community, Professional, or Enterprise).

3. Xamarin for Visual Studio.

For more information about the pre-requisites for installing
and using Xamarin, see 
[System Requirements](~/cross-platform/get-started/requirements.md).

<a name="installation" />

## Installation

Xamarin can be installed as part of a new Visual Studio 2017 installation.
To achieve this, use the following steps:

1. Download Visual Studio 2017 Community, Visual Studio Professional, or
   Visual Studio Enterprise from the
   [Visual Studio](https://www.visualstudio.com/vs/) page (download
   links are provided at the bottom).

2. Double-click the downloaded package to start installation.

3. Select the **Mobile development with .NET** workload from the
   installation screen: 

    [![Mobile development with .NET selection on the Workloads screen](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png#lightbox)

4. While **Mobile development with .NET** is selected, have a look at
   the **Summary** panel on the right. Here, you can deselect mobile
   development options that you do not want to install. By default, all
   options shown in the following screenshot are installed (**Xamarin
   Workbooks**, **Xamarin Profiler**, **Xamarin Remoted Simulator**,
   **Android NDK**, **Android SDK**, **Java SE Development Kit**,
   **Google Android Emulator**, **F# support**, and **Intel HAXM**):

    ![Summary panel listing Xamarin options to install](windows-images/02-summary.png)

5. When you are ready to begin Visual Studio 2017 installation, click the
   **Install** button in the lower right-hand corner:

    ![Location of installation button](windows-images/03-click-install.png)

   Depending on which edition of Visual Studio 2017 you are installing, the
   installation process can take a long time to complete. You can use
   the progress bars to monitor the installation:

    ![Example screenshot of progress bars during installation](windows-images/04-progress-bars.png)

6. When Visual Studio 2017 installation has completed, click the **Launch**
   button to start Visual Studio:

    ![Location of Launch button](windows-images/05-launch.png)

<a name="vs2017" />

### Adding Xamarin to Visual Studio 2017

If Visual Studio 2017 is already installed, you can add Xamarin by
re-rerunning the Visual Studio 2017 installer to modify workloads (see
[Modify Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio)
for details). Next, follow the steps listed above to install Xamarin.

For more information about downloading and installing Visual Studio
2017, see [Install Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


### Verifying Installation

In Visual Studio 2017, you can verify that Xamarin is installed by 
clicking the **Help** menu. If Xamarin is installed, you should
see a **Xamarin** menu item as shown in this screenshot:

![Xamarin menu item displayed on the Help menu](windows-images/12-xamarin-menu-item.png)

If you are using an earlier versions of Visual Studio, you can click
**Help > About Microsoft Visual Studio** and scroll through the list of
installed products to see if Xamarin is installed:

![Visual Studio installed products screen](windows-images/13-xamarin-is-installed.png)

For more information about locating version information, see
[Where can I find my version information and logs?](~/cross-platform/troubleshooting/questions/version-logs.md)

<a name="nextsteps" />

## Next Steps

Installing Xamarin in Visual Studio 2017 allows you to start writing code
for your apps, but does require additional setup for building and
deploying your apps to simulator, emulator, and device. Visit the
following guides to complete your installation and start building cross
platform apps.

### iOS

For more detailed information, see the [Installing Xamarin.iOS on Windows](~/ios/get-started/installation/windows/index.md) guide. 

1. [Install Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/installation)
2. [Connect Visual Studio to your Mac build host](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
3. [iOS Developer Setup](~/ios/get-started/installation/device-provisioning/index.md) - Required to run your application on device
5. [Remoted iOS Simulator](~/tools/ios-simulator.md)
6. [Introduction to Xamarin.iOS for Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### Android

For more detailed information, see the [Installing Xamarin.Android on Windows](~/android/get-started/installation/windows.md) guide.

1. [Xamarin.Android Configuration](~/android/get-started/installation/windows.md#configuration)
2. [Using the Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Android SDK Emulator](~/android/get-started/installation/android-emulator/index.md)
4. [Set Up Device for Development](~/android/get-started/installation/set-up-device-for-development.md)
