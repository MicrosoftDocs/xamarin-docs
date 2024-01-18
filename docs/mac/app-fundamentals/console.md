---
title: "Using Xamarin.Mac bindings for Console Apps"
description: "Create a headless console app using Xamarin.Mac to access native macOS APIs."
ms.service: xamarin
ms.assetid: 9E52B4CC-F530-4B3E-984A-7F5719A6D528
ms.subservice: xamarin-mac
author: migueldeicaza
ms.author: miguel
ms.date: 07/27/2018
no-loc: [Objective-C]
---
# Xamarin.Mac bindings in console apps

There are some scenarios where you want to use some of Apple native APIs in C# to build a headless application &ndash; one that does not have a user interface &ndash; using C#.

The project templates for Mac applications include a call to `NSApplication.Init()` followed by a call to `NSApplication.Main(args)`, it usually looks like this:

```csharp
static class MainClass {
    static void Main (string [] args)
    {
        NSApplication.Init ();
        NSApplication.Main (args);
    }
}
```

The call to `Init` prepares the Xamarin.Mac runtime, the call to `Main(args)` starts the Cocoa application main loop, which prepares the application to receive keyboard and mouse events and show the main window of your application.   The call to `Main` will also attempt to locate Cocoa resources, prepare a toplevel window and expects the program to be part of an application bundle (programs distributed in a directory with the `.app` extension and a very specific layout).

Headless applications do not need a user interface, and do not need to run as part of an application bundle.

## Creating the console app

So it is better to start with a regular .NET Console project type.

You need to do a few things:

- Create an empty project.
- Reference the Xamarin.Mac.dll library.
- Bring the unmanaged dependency to your project.

These steps are explained in more detail below:

### Create an empty Console Project

Create a new .NET Console Project, make sure that it is .NET and not .NET Core, as Xamarin.Mac.dll does not run under the .NET Core runtime, it only runs with the Mono runtime.

### Reference the Xamarin.Mac library

To compile your code, you will want to reference the `Xamarin.Mac.dll` assembly from this directory:
`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib/64bits/full`

To do this, go to the project references, select the **.NET Assembly** tab, and click the **Browse** button to locate the file on the file system.  Navigate to the path above, and then select the **Xamarin.Mac.dll** from that directory.

This will give you access to the Cocoa APIs at compile time.   At this point, you can add `using AppKit` to the top of your file, and call the `NSApplication.Init()` method.   There is only one more step before you can run your application.

### Bring the unmanaged support library into your project

Before your application will run, you need to bring the `Xamarin.Mac` support library into your project.   To do this, add a new file to your project (in project options, select **Add**, and then **Add Existing File**) and navigate to this directory:

`/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/SDKs/Xamarin.macOS.sdk/lib`

Here, select the file **libxammac.dylib**.   You will be offered a choice of copying, linking or moving.   I personally like linking, but copying works as well.    Then you need to select the file, and in the property pad (select **View>Pads>Properties** if the property pad is not visible), go to the **Build** section and set the **Copy to Output Directory** setting to **Copy if newer**.

You can now run your Xamarin.Mac application.

The result in your bin directory will look like this:

```
Xamarin.Mac.dll
Xamarin.Mac.pdb
consoleapp.exe
consoleapp.pdb
libxammac.dylib
```

To run this app, you will need all those files in the same directory.

## Building a standalone application for distribution

You might want to distribute a single executable to your users.  To do this, you can use the `mkbundle` tool to turn the various files into a self-contained executable.

First, make sure that your application compiles and runs.   Once you are satisfied with the results, you can run the following command from the command line:

```
$ mkbundle --simple -o /tmp/consoleapp consoleapp.exe --library libxammac.dylib --config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/config --machine-config /Library/Frameworks/Mono.framework/Versions/Current/etc/mono/4.5/machine.config
[Output from the bundling tool]
$ _
```

In the above command line invocation, the option `-o` is used to specify the generated output, in this case, we passed `/tmp/consoleapp`.   This is now a standalone application that you can distribute and has no external dependencies on Mono or Xamarin.Mac, it is a fully self contained executable.

The command line manually specified the **machine.config** file to use, and a system-wide library mapping configuration file.   They are not necessary for all applications, but it is convenient to bundle them, as they are used when you use more capabilities of .NET

## Project-less builds

You do not need a full project to create a self-contained Xamarin.Mac application, you can also use simple Unix makefiles to get the job done.   The following example shows how you can setup a makefile for a simple command line application:

```
XAMMAC_PATH=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib/x86_64/full/
DYLD=/Library/Frameworks/Xamarin.Mac.framework/Versions/Current/lib
MONODIR=/Library/Frameworks/Mono.framework/Versions/Current/etc/mono

all: consoleapp.exe

consoelapp.exe: consoleapp.cs Makefile
    mcs -g -r:$(XAMMAC_PATH)/Xamarin.Mac.dll consoleapp.cs
    
run: consoleapp.exe
    MONO_PATH=$(XAMMAC_PATH) DYLD_LIBRARY_PATH=$(DYLD) mono --debug consoleapp.exe $(COMMAND)

bundle: consoleapp.exe
    mkbundle --simple consoleapp.exe -o ncsharp -L $(XAMMAC_PATH) --library $(DYLD)/libxammac.dylib --config $(MONODIR)/config --machine-config $(MONODIR)/4.5/machine.config
```

The above `Makefile` provides three targets:

- `make` will build the program
- `make run` will build and run the program in the current directory
- `make bundle` will create a self-contained executable
