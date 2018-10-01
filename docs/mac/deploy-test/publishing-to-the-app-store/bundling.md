---
title: "Bundling for the Mac App Store"
description: "This document describes how to bundle a Xamarin.Mac app for publication to the Mac App Store. It discusses code signing options and building."
ms.prod: xamarin
ms.assetid: 00a36d7c-937d-4657-bf6a-0de9684b8f94
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
---

# Bundling for the Mac App Store

This section describes the basics of building an application for release in the Mac App Store using Visual Studio for Mac. Based on additional features (such as iCloud access and push notifications), further setup may be required that goes beyon the scope of this article.

> [!NOTE]
> Before starting this section, the developer must have created a production provisioning profile to build for the Mac App Store. See the instructions earlier in this document about creating the required provisioning profiles.

## Code signing options

Change the **Configuration** to **Release** before updating the code signing and packaging options. The developer needs to make sure that they use their company **Identity** and the provisioning profile that we created above when signing the application for release in the App Store.

 [![Editing the code signing options](bundling-images/config02.png "Editing the code signing options")](bundling-images/config02-large.png#lightbox)

Ensure that the option to create an installer package has been checked in the **Mac Build** settings:

[![Editing the build options](bundling-images/config03.png "Editing the build options")](bundling-images/config03-large.png#lightbox)

## Build

Before building, ensure that the **Release** configuration has been selected. When the developer builds the app, they’ll be prompted to use both certificates:

 ![Allowing the app to use the certificate](bundling-images/image62.png "Allowing the app to use the certificate")

 ![Allowing the app to use the certificate](bundling-images/image63.png "Allowing the app to use the certificate")

After the application has been built, the developer can right-click on the project and choose **Open Containing Folder** to find the package file (in the `bin/x86/AppStore` directory in the example shown below).  This package file includes an installer for the app that can be submitted to Apple for inclusion in the Mac App Store.

 ![Selecting the build package in Finder](bundling-images/image64.png "Selecting the build package in Finder")


## Related Links

- [Installation](/visualstudio/mac/installation/)
- [Hello, Mac sample](~/mac/get-started/hello-mac.md)
- [Distribute your apps on the Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID and GateKeeper](https://developer.apple.com/resources/developer-id/)
