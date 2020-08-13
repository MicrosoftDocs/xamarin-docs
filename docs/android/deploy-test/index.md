---
title: "Testing, Optimizing, and Deploying Xamarin.Android Apps"
description: "This section includes guides that explain how to test an application, optimize its performance, prepare it for release, sign it with a certificate, and publish it to an app store"
ms.prod: xamarin
ms.assetid: 568C0B85-EFF3-AF6F-5605-95055193D367
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
---

# Deployment and Testing of Xamarin.Android Apps

This section includes guides that explain how to test an application,
optimize its performance, prepare it for release, sign it with a
certificate, and publish it to an app store.

## [Application Package Sizes](app-package-size.md)

This article examines the constituent parts of a Xamarin.Android
application package and the associated strategies that can be used for
efficient package deployment during debug and release stages of
development.

## [Apply Changes](apply-changes.md)

This guide covers the Apply Changes feature which lets you push
resource changes to your running app without restarting your app.

## [Building Apps](building-apps/index.md)

This section describes how the build process works and explains how to
build ABI-specific APKs.

## [Command Line Emulator](command-line-emulator.md)

This article briefly touches starting the emulator via the command
line.

## [Debugging](~/android/deploy-test/debugging/index.md)

The guides in the section help you to debug your app using Android emulators,
real Android devices, and the debug log.

## [Setting the Debuggable Attribute](~/android/deploy-test/debuggable-attribute.md)

This article explains how to set the debuggable attribute so that tools
such as `adb` can communicate with the JVM.

## [Environment](environment.md)

This article describes the Xamarin.Android execution environment and
the Android system properties that influence program execution.

## [GDB](gdb.md)

This article explains how to use `gdb` for debugging a Xamarin.Android
application.

## [Installing a System App](install-system-app.md)

This guide explains how to install a Xamarin.Android app as a
System Application on an Android device or as part of a custom ROM.

## [Linking on Android](linker.md)

This article discusses the linking process used by Xamarin.Android to
reduce the final size of an application. It describes the various
levels of linking that can be performed and provides some guidance and
troubleshooting advice to mitigate errors that might result from using
the linker.

## [Xamarin.Android Performance](~/android/deploy-test/performance.md)

There are many techniques for increasing the performance of
applications built with Xamarin.Android. Collectively these techniques
can greatly reduce the amount of work being performed by a CPU and the
amount of memory consumed by an application.

## [Profiling Android Apps](~/android/deploy-test/profiling.md)

This guide explains how to use profiler tools to examine the
performance and memory usage of an Android app.

## [Preparing an Application for Release](~/android/deploy-test/release-prep/index.md)

After an application has been coded and tested, it is necessary to
prepare a package for distribution. The first task in preparing
this package is to build the application for release, which mainly entails
setting some application attributes.

## [Signing the Android Application Package](~/android/deploy-test/signing/index.md)

Learn how to create an Android signing identity, create a new signing
certificate for Android applications, and sign the application with the
signing certificate. In addition, this topic explains how to export the
app to disk for *ad-hoc* distribution. The resulting APK can be
sideloaded into Android devices without going through an app store.

## [Publishing an Application](~/android/deploy-test/publishing/index.md)

This series of articles explains the steps for public distribution of
an application created with Xamarin.Android. Distribution can take
place via channels such as e-mail, a private web server, Google Play,
or the Amazon App Store for Android.
