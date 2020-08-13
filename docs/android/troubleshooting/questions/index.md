---
title: "Xamarin.Android Frequently Asked Questions"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 0F0FDD2B-FFB1-476F-B674-81DB3A5E1CF3
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
---

# Android Frequently Asked Questions

## Installation & Setup

### [Which Android SDK packages should I install?](install-android-sdk-packages.md)

Installing the Android SDK doesn't automatically include all the
minimum required packages for developing. While individual developer
needs vary, this guide discusses the packages that will generally be
required for developing with Xamarin.Android.

### [Where can I set my Android SDK locations?](android-sdk-location.md)

This guide describes both the default settings of the Android SDK,
which should work for most setups; and how to change these defaults in
Visual Studio for Mac or Visual Studio if needed.

### [How do I update the Java Development Kit (JDK) version?](update-jdk.md)

This article illustrates how to update the Java Development Kit (JDK)
version on Windows and Mac.

### [Can I use Java Development Kit (JDK) version 9 or later?](jdk9-errors.md)

Xamarin.Android requires JDK 8 or the Microsoft Mobile OpenJDK. This
article lists some common error messages that you may see if JDK 9 or
later is installed, along with instructions for checking the JDK
version.

### [How can I manually install the Android Support libraries required by the Xamarin.Android.Support packages?](install-android-support-library.md)

This guide provides example steps for installing the
`Xamarin.Android.Support.v4` support library on Windows & Mac.

### [What USB drivers do I need to debug Android on Windows?](android-drivers-debug-windows.md)

To debug on an Android device when developing in Windows; you need to
install a compatible USB driver. The Android SDK Manager includes the
"Google USB Driver" by default, which adds support for Nexus devices.
Other devices require USB drivers published by the device
manufacturer. This guide provides information on finding these drivers
as well as alternative testing methods.

### [Is it possible to connect to Android emulators running on a Mac from a Windows VM?](connect-android-emulator-mac-windows.md)

This guide covers methods when using the Android emulator.

## General Questions

### [How do I automate an Android NUnit Test project?](automate-android-nunit-test.md)

This guide covers steps for setting up an Android NUnit test project,
_not_ a Xamarin.UITest project. Xamarin.UITest guides can be found
[here](/appcenter/test-cloud/preparing-for-upload).

### [Why can't my Android release build connect to the Internet?](android-internet.md)

The most common cause of this issue is that the **INTERNET** permission
is automatically included in a debug build, but must be set manually
for a release build. This guide describes how to enable the permission
on release builds.

### [Smarter Xamarin Android Support v4 / v13 NuGet Packages](android-support-v4v13-libraries.md)

`Support-v4` and `Support-v13` can not be used together in the same
app, that is, they are mutually exclusive. This is because
`Support-v13` actually contains all of the types and implementation of
`Support-v4`. If you try and reference both in the same project, you
will encounter duplicate type errors.

### [How do I resolve a PathTooLongException Error?](path-too-long-exception.md)

This article explains how to resolve a **PathTooLongException** error
that may occur while building a Xamarin.Android project.

## Deprecated

> [!NOTE]
> The articles below apply to issues that have been
resolved in recent versions of Xamarin. However, if the issue occurs on
the latest version of the software, please file a
[new bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md)
with your full versioning information and full build log output.

### [What version of Xamarin.Android added Lollipop support?](xa-lollipop.md)

This guide was originally written for the Android L
preview.Xamarin.Android 4.17 added Android L Preview Support &
Xamarin.Android 4.20 added Android Lollipop Support.

### [Android.Support.v7.AppCompat - No resource found that matches the given name: attr 'android:actionModeShareDrawable'](missing-action-mode-share-drawable.md)

This error may occur in older versions of Xamarin if some of the
required Android SDK packages are missing.

### [Adjusting Java memory parameters for the Android designer](android-designer-java-memory.md)

The default memory parameters that are used when starting the `java`
process for the Android designer might be incompatible with some system
configurations. Starting with Xamarin Studio 5.7.2.7 and Xamarin for
Visual Studio 3.9.344 these settings can be customized on a per-project
basis.

### [My Android Resource.designer.cs file will not update](resource-designer-wont-update.md)

A bug in Xamarin.Studio 5.1 previously corrupted .csproj files by
partially or completely deleting the xml code in the .csproj file. This
would cause important parts of the Android build system (such as
updating the Android Resource.designer.cs) to fail. As of the 5.1.4
stable release on July 15th, this bug has been fixed; but in many cases
the project file has to be repaired manually, as described in this
guide.
