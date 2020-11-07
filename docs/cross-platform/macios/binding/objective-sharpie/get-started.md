---
title: "Getting Started With Objective Sharpie"
description: "This document provides a high-level overview of Objective Sharpie, the tool used to automate the creation of C# bindings to Objective-C code."
ms.prod: xamarin
ms.assetid: 577512BF-1A90-41E5-89DE-9E056C478678
author: davidortinau
ms.author: daortin
ms.date: 10/11/2017
---

# Getting Started With Objective Sharpie

> [!IMPORTANT]
> Objective Sharpie is a tool for experienced Xamarin developers with
> advanced knowledge of Objective-C (and by extension, C). Before
> attempting to bind an Objective-C library you should have solid
> knowledge of how to build the native library on the command line (and a
> good understanding of how the native library works).

<a name="installing"></a>

## Installing Objective Sharpie

Objective Sharpie is currently a standalone command line tool for Mac OS X 10.10
and newer, and is _not a fully supported Xamarin product_. It should only be
used by advanced developers to assist in creating a binding project to a 3rd
party Objective-C library.

Objective Sharpie can be downloaded as a standard OS X package installer.
Run the installer and follow all of the on-screen prompts from the installation wizard:

- **Current Version: 3.4**
  - [Download Latest Release](https://aka.ms/objective-sharpie)
  - [Forum Announcement](https://forums.xamarin.com/discussion/104800/objective-sharpie-3-4)

> [!TIP]
> Use the `sharpie update` command to update to the latest version.

## Basic Walkthrough

Objective Sharpie is a command line tool provided by Xamarin that assists in
creating the definitions required to bind a 3rd party Objective-C library to C#.
Even when using Objective Sharpie, the developer *will* need to modify the
generated files after Objective Sharpie finishes to address any issues that could
not be automatically handled by the tool.

Where possible, Objective Sharpie will annotate APIs with which it has some
doubt on how to properly bind (many constructs in the native code are ambiguous).
These annotations will appear as [`[Verify]` attributes](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md).

The output of Objective Sharpie is a pair of files -
[`ApiDefinition.cs` and `StructsAndEnums.cs`](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) -
that can be used to create a binding project which compiles into a library
you can use in Xamarin apps.

> [!IMPORTANT]
> Objective Sharpie comes with one **major** rule for proper usage: you
> must absolutely pass it the correct clang compiler command line arguments
> in order to ensure proper parsing. This is because the Objective Sharpie
> parsing phase is simply a tool [implemented against the clang libtooling
> API](https://clang.llvm.org/docs/LibTooling.html).

This means that Objective Sharpie has the full power of Clang
(the C/Objective-C/C++ compiler that actually compiles the native library
you would bind) and all of its internal knowledge of the header files for binding.
Instead of translating the parsed [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree)
to object code, Objective Sharpie translates the AST to a C# binding "scaffold"
suitable for input to the `bmac` and `btouch` Xamarin binding tools.

If Objective Sharpie errors out during parsing, it means that clang errored out
during its parsing phase trying to construct the AST, and you need to figure out why.

**NEW!** version 3.0 attempts address some of this complexity by supporting
Xcode projects directly. If a native library has a valid Xcode project,
Objective Sharpie can evaluate the project for a specified target and
configuration to deduce the necessary input header files and compiler flags.

If no Xcode project is available, you will need to be more familiar with the
project by deducing the correct input header files, header file search paths,
and other necessary compiler flags. It is important to realize that the
compiler flags used to build the native library are the same that must be
passed to Objective Sharpie. This is a more manual process, and one that
does require a bit of familiarity with compiling native code on the command
line with the Clang toolchain.

**NEW!** version 3.0 also introduces a tool for easily binding
[CocoaPods](https://cocoapods.org) via the `sharpie pod` command.
If the library you're interested in is available as a CocoaPod,
we recommend you start by attempting to bind the CocoaPod with
Objective Sharpie (versus attempting to bind against the source directly).
