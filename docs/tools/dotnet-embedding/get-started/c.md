---
title: "Getting started with C"
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
---

# Getting started with C


## Requirements

To use .NET Embedding with C you'll need a Mac or Windows machine running:

* macOS 10.12 (Sierra) or later
* Xcode 8.3.2 or later

* Windows 7, 8, 10 or later
* Visual Studio 2015 or later

* [Mono](http://www.mono-project.com/download/)


## Installation

Your next step is to download and install the .NET Embedding tools on your machine.

Binary builds for the C and Java generators are still not available but are coming soon.

As an alternative it is possible to build it from our git repository, see the [contributing](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) document for instructions.


## Generation

To generate C code, invoke the .NET Embedding tool passing the right flags to target the C language:

### Windows:

```csharp
$ build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=windows --compile managed.dll
```

Make sure to call from a Visual Studio command shell specific to the Visual Studio version you're targetting.

### macOS

```csharp
$ mono build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=macos --compile managed.dll
```

### Output files

If all goes well, you will be presented with the following output:

```csharp
Parsing assemblies...
    Parsed 'managed.dll'
Processing assemblies...
Generating binding code...
    Generated: managed.h
    Generated: managed.c
    Generated: mscorlib.h
    Generated: mscorlib.c
    Generated: embeddinator.h
    Generated: glib.c
    Generated: glib.h
    Generated: mono-support.h
    Generated: mono_embeddinator.c
    Generated: mono_embeddinator.h
```

Since the `--compile` flag was passed to the tool, .NET Embedding should also have compiled the output files into a shared library, which you can find next to the generated files, a `libmanaged.dylib` file on macOS, and `managed.dll` on Windows.

To consume the shared library you can include the `managed.h` C header file, which provides the C declarations corresponding to the respective managed library APIs and link with the previously mentioned compiled shared library.

## Further Reading

* [Embeddinator Limitations](~/tools/dotnet-embedding/limitations.md)
* [Contributing to the open source project](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Error codes and descriptions](~/tools/dotnet-embedding/errors.md)
