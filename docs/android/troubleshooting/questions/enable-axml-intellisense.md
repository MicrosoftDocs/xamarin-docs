---
title: "How do I enable Intellisense in Android .axml files?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 84850CB2-1CE2-4D3F-BD01-6B3B033F5A4C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
---

# How do I enable Intellisense in Android .axml files?

Android Intellisense in Visual Studio needs two XML Schema files to work appropriately. To find these files, go to this folder:

`C:\Program Files (x86)\Xamarin Studio\AddIns\MonoDevelop.MonoDroid\schemas`*

*(Previously: `C:\Program Files (x86)\MSBuild\Xamarin\Android`)

Inside you will find two .xsd files:

1. android-layout-xml.xsd
2. schemas.android.com.apk.res.android.xsd

Visual Studio keeps all of the XML Schemas inside the respective folder:

`C:\Program Files (x86)\Microsoft Visual Studio 12.0\Xml\Schemas`

You can choose to move these two .xsd files to the location above, or simply just add these schemas within Visual Studio. You can then "Add" these schemas inside Visual Studio via the **XML > Schemas > Add** dialog.






