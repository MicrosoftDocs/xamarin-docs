---
title: "Xamarin.Mac ahead of time compilation"
description: "This document describes ahead of time compilation in Xamarin.Mac. It compares AOT compilation to JIT compilation, explains how to enable AOT, and takes a look at hybrid AOT."
ms.prod: xamarin
ms.assetid: 38B8A017-5A58-429C-A6E9-9860A1DCEF63
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 11/10/2017
---

# Xamarin.Mac ahead of time compilation

## Overview

Ahead of time (AOT) compilation is a powerful optimization technique for improving startup performance. However, it also affects your build time, application size, and program execution in profound ways. To understand the tradeoffs it imposes, we’re going to dive a bit into the compilation and execution of an application.

Code written in managed languages, such as C# and F#, is compiled to an intermediate representation called IL. This IL, stored in your library and program assemblies, is relatively compact and portable between processor architectures. IL, however, is only an intermediate set of instructions and at some point that IL will need to be translated into machine code specific to the processor.

There are two points in which this processing can be done:

- **Just in time (JIT)** – During startup and execution of your application the IL is compiled in memory to machine code.
- **Ahead of time (AOT)** – During build the IL is compiled and written out to native libraries and stored within your application bundle.

Each option has a number of benefits and tradeoffs:

- **JIT**
  - **Startup Time** – JIT compilation must be done on startup. For most applications this is on the order of 100ms, but for large applications this time can be significantly more.
  - **Execution** – As the JIT code can be optimized for the specific processor being used, slightly better code can be generated. In most applications this is a few percentage points faster at most.
- **AOT**
  - **Startup Time** – Loading pre-compiled dylibs is significantly faster than JIT assemblies.
  - **Disk Space** – Those dylibs can take a significant amount of disk space however. Depending on which assemblies are AOTed, it can double or more the size of the code portion of your application.
  - **Build Time** – AOT compilation is significantly slower that JIT and will slow builds using it. This slowdown can range from seconds up to a minute or more, depending on the size and number of assemblies compiled.
  - **Obfuscation** – As the IL, which is significantly easier to reverse engineer than machine code, is not necessarily required it can be stripped to help obfuscate sensitive code. This requires the "Hybrid” option describe below.

## Enabling AOT

AOT options will be added to the Mac Build pane in a future update. Until then, enabling AOT requires passing a command line argument via the “Additional mmp arguments” field in Mac Build. The options are as follows:


      --aot[=VALUE]          Specify assemblies that should be AOT compiled
                               - none - No AOT (default)
                               - all - Every assembly in MonoBundle
                               - core - Xamarin.Mac, System, mscorlib
                               - sdk - Xamarin.Mac.dll and BCL assemblies
                               - |hybrid after option enables hybrid AOT which
                               allows IL stripping but is slower (only valid
                               for 'all')
                                - Individual files can be included for AOT via +
                               FileName.dll and excluded via -FileName.dll

                               Examples:
                                 --aot:all,-MyAssembly.dll
                                 --aot:core,+MyOtherAssembly.dll,-mscorlib.dll



## Hybrid AOT

During execution of a macOS application the runtime defaults to using machine code loaded from the native libraries produced by AOT compilation. There are, however, some areas of code such as trampolines, where JIT compilation can produce significantly more optimized results. This requires the managed assemblies IL to be available. On iOS, applications are restricted from any use of JIT compilation; those section of code are AOT compiled as well.

The hybrid option instructs the compiler to both compile these section (like iOS) but also to assume that the IL will not be available at runtime. This IL can then be stripped post build. As noted above, the runtime will be forced to use less optimized routines in some places.

## Further considerations

The negative consequences of AOT scale with the sizes and number of assemblies processed. The Full [target framework](~/mac/platform/target-framework.md) for example contains a significantly larger Base Class Library (BCL) than Modern, and thus AOT will take significantly longer and produce larger bundles. This is compounded by the Full target framework’s incompatibility with Linking, which strips out unused code. Consider moving your application to Modern and enabling Linking for the best results.

One additional benefit of AOT comes with improved interactions with native debugging and profiling toolchains. Since a vast majority of the codebase will be compiled ahead of time, it will have function names and symbols that are easier to read inside native crash reports, profiling, and debugging. JIT generated functions do not have these names and often show up as unnamed hex offsets that are very difficult to resolve.
