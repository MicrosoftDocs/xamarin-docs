---
title: "Execution Environment for Xamarin.iOS Apps"
description: "This document describes how to set up temporary and permanent environment variables for a Xamarin.iOS app. The variables can be specified in a project's properties or as extra arguments to the mtouch packaging tool."
ms.prod: xamarin
ms.assetid: 9801644A-89BB-4491-AD28-7F3B97D2CD62
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
---

# Execution Environment for Xamarin.iOS Apps

The *execution environment* is the set of environment variables that influence program
execution. Environment variables can be set temporarily in the project's properties, or
permanently by specifying extra arguments to the mtouch packaging tool.

## Temporary environment variables

Temporary environment variables
are set in the project's **Properties**/**Options** window
in the **Run > General** section. These environment variables
are only in effect when the application is started using Visual Studio for Mac, if
the app is started manually by tapping on it these environment variables are
not set.

## Permanent environment variables

Permanent environment variables are set by specifying extra arguments to
the mtouch packaging tool. These environment variables are compiled into the
executable, and will be set even if the app is not launched from Visual Studio for Mac.

## Example

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

