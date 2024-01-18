---
title: "Why doesn't Visual Studio include my referenced library project in my build?"
ms.topic: troubleshooting
ms.service: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
no-loc: [Objective-C]
description: Learn how to use Visual Studio to use the configuration manager to determine which projects in a solution are included in a build or development configuration.
---

# Why doesn't Visual Studio include my referenced library project in my build?

Visual Studio uses the **Configuration Manager** to determine which projects in a solution are automatically included in a given build or deployment configuration.

Some templates that are generated with a referenced library project will already have the referenced library included in the configuration; but otherwise it will need to be set manually.

## How to use the Configuration Manager

1. Open **Build > Configuration Manager**
2. Select the configuration to customize, e.g. **Debug | iPhone**
3. Select checkboxes for the projects you wish to include.

> [!NOTE]
> Greyed out boxes are handled automatically, and shouldn't need any changes.

Screencast of these steps: [https://screencast.com/t/zLoQOpEn](https://screencast.com/t/zLoQOpEn)
