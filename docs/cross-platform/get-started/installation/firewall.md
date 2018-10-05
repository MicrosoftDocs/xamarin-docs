---
title: "Xamarin Firewall Configuration Instructions"
description: "This document provides a list of hosts that must be whitelisted in your firewall to allow Xamarin to work in a corporate environment."
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
author: asb3993
ms.author: amburns
ms.date: 10/05/2018
---

# Xamarin Firewall Configuration Instructions

_A list of hosts that you need to whitelist in your firewall to allow Xamarin’s platform to work for your company._

In order for Xamarin products to install and work properly, certain endpoints must be accessible to download the required tools and updates for your software. If you or your company have strict firewall settings, you may experience issues with installation, licensing, components, and more. This document outlines some of the known endpoints that need to be whitelisted in your firewall in order for Xamarin to work. This list does not include the endpoints required for any third-party tools included in the download. If you are still experiencing trouble after going through this list, refer to the Apple or Android installation troubleshooting guides.

## Endpoints to Whitelist

### Xamarin Installer

The following known addresses will need to be added in order for the software to install properly when using the latest release of the Xamarin installer:

- xamarin.com (installer manifests)
- dl.xamarin.com (Package download location)
- dl.google.com (to download the Android SDK)
- download.oracle.com (JDK)
- visualstudio.com (Setup packages download location)
- go.microsoft.com (Setup URL resolution)
- aka.ms (Setup URL resolution)

If you are using a Mac and are encountering Xamarin.Android install issues, please ensure that macOS is able to download Java.

### NuGet (including Xamarin.Forms)

The following addresses will need to be added to access NuGet (Xamarin.Forms is packaged as a NuGet):

- www\.nuget.org (to access NuGet)
- az320820.vo.msecnd.net (NuGet downloads)
- dl-ssl.google.com (Google components for Android and Xamarin.Forms)

### Software updates

The following addresses will need to be added to ensure that software updates can download properly:

- software.xamarin.com (updater service)
- download.visualstudio.microsoft.com
- dl.xamarin.com

## Xamarin Mac Agent

To connect Visual Studio to a Mac build host using the Xamarin Mac Agent requires the SSH port to be open. By default this is **Port 22**.

## Summary

This guide covered the endpoints to whitelist to allow Xamarin products to install and update properly on your machine.
