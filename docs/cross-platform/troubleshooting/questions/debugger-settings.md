---
title: "What project settings are required for the debugger?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3A024E4E-ACA3-4C7A-ADEF-541665D15779
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
---

# What project settings are required for the debugger?

In order for the debugger to work as expected (hit breakpoints, display debug logs, etc.), developer instrumentation and debug information display must both be enabled.

Please follow these steps to check your environment settings:

## Visual Studio
1. Open the Project options
2. Go to **Build > Advanced...** Set Debug Information to **Full**
3. Settings for each platform:
   - Go to **Android Options > Debugging Options**. Tick the **Enable Developer Instrumentation** box.
   - Go to **iOS Debug > Debugging & Instrumentation**. Tick the **Enable Debugging** box.

## Visual Studio for Mac
1. Open the Project options
2. Go to **Build > Compiler > General Options**. Set Debug Information to **Full**
3. Settings for each platform:
  - Go to **Build > Android Build > Debugging Options**. Tick the **Enable Developer Instrumentation** box.
  - Go to **Build > iOS Debug**. Tick the **Enable Debugging** box.

