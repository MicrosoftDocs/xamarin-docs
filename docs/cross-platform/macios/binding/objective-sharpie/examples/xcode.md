---
title: "Real-World Example using an Xcode Project"
ms.topic: article
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
---

# Real-World Example using an Xcode Project


**This example uses the [POP library from Facebook](https://github.com/facebook/pop).**

New in version 3.0, Objective Sharpie supports Xcode projects as input. These projects specify the correct header files and compiler flags necessary to compile the native library, and thus necessary to bind it too. Objective Sharpie will select the first _target_ and its default configuration of a project if not instructed to do otherwise.

Before Objective Sharpie attempts to parse the project and header files, it must build it. Projects often have build phases that will correctly structure header files for external consumption and integration, so it is best to always build the full project before attempting to bind it.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

