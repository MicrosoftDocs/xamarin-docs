---
title: "Environment"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
---

# Environment

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
executable, and will be set even if the app is not launched from Xamarin
Studio.

## Example

```csharp
# log all exceptions to the device log
--setenv:MONO_TRACE=E:all

# to pass multipe environment variables, use --setenv multiple times
--setenv:MONO_TRACE=E:all --setenv:GC_DONT_GC=1
```

