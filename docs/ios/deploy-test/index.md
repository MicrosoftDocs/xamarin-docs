---
title: "Deploying and Testing Xamarin.iOS Apps"
description: "This document links to various guides that describe topics related to deploying and testing a Xamarin.iOS application. For example, app distribution, .ipa files, provisioning, wireless deployment, TestFlight, and debugging."
ms.prod: xamarin
ms.assetid: 2DBF3BF9-79E7-4E24-AF26-E34C972B0169
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
---

# Deploying and Testing Xamarin.iOS Apps

This section covers topics used to test an application as well as how to
distribute it. Topics here include things such as tools used for
debugging, deployment to testers and how to publish an application to the App
Store.

##  [App Distribution](~/ios/deploy-test/app-distribution/index.md)

This article shows how to configure, build, and publish a Xamarin.iOS
application for distribution through various different means, including:

- [App Store Distribution](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [In-House (Enterprise) Distribution](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Ad Hoc Distribution](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)

##  [IPA Deployment](~/ios/deploy-test/app-distribution/ipa-support.md)

Ad-Hoc and Enterprise deployments allow developers to create packages that
can be distributed for testing or to internal company users. This document
explains how to create an IPA that can be synced to an iOS
device using iTunes.

## [Provisioning](provisioning/index.md)

This set of guides covers code signing and provisioning essentials such as working with property lists, and how to provision your app for application services. 

## [Wireless Deployment](wireless-deployment.md)

 Xcode 9 introduced the option of deploying to an iOS device or Apple TV through a network, rather than having to hardwire your devices every time you want to deploy and debug your app. This feature is currently in preview.

##  [TestFlight](~/ios/deploy-test/testflight.md)

TestFlight is now owned by Apple, and is the primary way to beta test your Xamarin.iOS apps. This article will guide you through all steps of the TestFlight Process – from uploading your app, to working with iTunes Connect.

##  [Debugging in Xamarin.iOS](~/ios/deploy-test/debugging-in-xamarin-ios.md)

Both the Visual Studio and Visual Studio for Mac IDEs include support for debugging Xamarin.iOS
applications both in the iOS simulator and on iOS devices. This article shows
how to use the debugger as well as how to configure various options it
supports.

##  [Touch.Unit](~/ios/deploy-test/touch.unit.md)

This document describes how to create unit tests for your Xamarin.iOS projects.
Unit testing with Xamarin.iOS is done using the Touch.Unit framework that includes
both an iOS test runner as well as a modified version of the [NUnitLite](http://www.nunitlite.com/) framework
that provides a familiar set of APIs for writing unit tests.

##  [Using Instruments to Detect Native Leaks using MarkHeap](~/ios/deploy-test/using-instruments-to-detect-native-leaks-using-markheap.md)

This article describe how to use Instruments on any iOS device
and any Xamarin.iOS application. It also looks at how to profile applications
in the simulator.

##  [Walkthrough - Using Apple's Instrument Tool](~/ios/deploy-test/walkthrough-apples-instrument.md)

This article walks through how to use Apple’s Instruments tool to diagnose memory issues in an iOS application built with Xamarin. It demonstrates how to launch Instruments, take heap snapshots and analyze memory growth. It also shows how to use Instruments to display and pinpoint the exact lines of code that cause the memory issue.

##  [Linking on iOS](linker.md)

Explains how the linker works to ensure the smallest possible application
package, as well as how to modify its settings and usage.

##  [Xamarin.iOS Performance](performance.md)

There are many techniques for increasing the performance of applications built with Xamarin.iOS. Collectively these techniques can greatly reduce the amount of work being performed by a CPU, and the amount of memory consumed by an application.

##  [mtouch](mtouch.md)

Notes and information on mtouch.exe, the command line tool that builds your
project into an application usable by iOS.

## [iOS Build Mechanics](ios-build-mechanics.md)

This guide explores how to time your apps and how to use methods that can be employed for quicker builds for all build configurations.