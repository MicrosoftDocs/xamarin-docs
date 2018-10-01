---
title: "fastlane for iOS – cert"
description: "This document describes fastlane, a tool that automates many parts of the iOS application provisioning process: requesting certificates, adding a device to Apple's Developer Portal, creating an App ID, and more."
ms.prod: xamarin
ms.assetid: 900FA6FF-F3C9-4D35-993E-B0D88E6B1883
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
---

# fastlane for iOS – cert

> [!IMPORTANT]
> fastlane recommends using the [`match`](~/ios/deploy-test/provisioning/fastlane/match.md) tool for generating and maintaining  certificates. Use `cert` directly only if you want full control and know enough about code signing. Use this action to download the latest code signing identity.

## Overview

Traditionally, device provisioning is performed by each member of a development team via Xcode or on the Apple Developer Portal. It consists of several steps:

- Requesting a development certificate
- Adding a device to the portal
- Creating an App ID
- Creating a provisioning profile
- Downloading profiles and certificates

Each of these steps contains variables that need to be addressed, which depend on the type of application you are developing. More information on the steps required to set up a device for development either manually or via Xcode can be found in the [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) guide.

This guide introduces fastlane tools as an alternative to using Xcode, and explains the following:

- [What is cert?](#whatiscert)
- [Using cert](#using)
- [Additional Options](#options)

## Installation

For information on installing and updating fastlane, refer to the Introduction to [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation) guide.

<a name="whatiscert" />

## What is cert?

cert provides a terminal interface that creates new code signing identities (often known as a developer _certificate_) for both development and distribution environments.

<a name="using" />

## Using cert

To use the cert utility, enter the following command into the terminal CLI:

    fastlane cert

By default, this will create a distribution certificate. To create a development certificate, pass the `--development` flag:

    fastlane cert --development

cert will prompt for your Apple ID and password, so enter this now:

[![](cert-images/fastlane-image1.png "cert will prompt for your Apple ID and password")](cert-images/fastlane-image1.png#lightbox)

> [!IMPORTANT]
> The first time your password is entered it is saved to the local macOS Keychain. Alternatively, Environment Variables can be used to store the username and password, or you can use `export fastlane_DONT_STORE_PASSWORD=1` if you do not wish to have your password stored in the keychain. For more information on managing credentials with fastlane, refer to fastlane's [credentials manager guide](https://github.com/fastlane/fastlane/blob/master/credentials_manager/README.md).

The Apple ID can also be passed as an argument by using the following command:

    fastlane cert -u myemailadress@domain.com

If your Apple ID is connected to multiple teams, they will be displayed here. Select the number that corresponds to the team that you wish to use:

[![](cert-images/fastlane-image2.png "Select the team that you wish to use")](cert-images/fastlane-image2.png#lightbox)

The Team ID can also be passed by using the following flag:

    fastlane cert -l 2TU993NY9J

fastlane will check if any of the available signing certificates is installed on your local machine, and if there is it will use it.

If there is no signing certificate, cert will:

- Create a new private key and signing request
- Generate, download, and install the certificate
- Import the certificate and private key into the keychain

When the max number of signing identities allowed for your account has been reached, an exception will be raised. If you wish to create a new signing identity, you must manually revoke one of the existing certificates via the Developer Center and try again.

> [!NOTE]
> fastlane cannot download existing signing identities from the Developer Center if they are not already in your keychain. This is because the private key only ever exists on your computer, or in exported(*.p12) version of the certificate, and never in the developer center.

<a name="options" />

## Additional Options

The following options can be used to give additional support when using cert:

- Use the `-–help` flag for a list of all available commands:

        fastlane cert --help

- Use the `-–verbose` flag to increase the verbosity of the output

        fastlane cert --development --verbose


## Related Links

- [fastlane – cert](https://github.com/fastlane/fastlane/blob/master/cert/README.md)
