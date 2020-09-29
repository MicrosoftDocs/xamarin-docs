---
ms.assetid: 814857C5-D54E-469F-97ED-EE1CAA0156BB
title: "Desktop app porting guidance"
description: "A simple explanation of how to decouple existing Windows Forms or WPF apps to create cross-platform apps to run on macOS, iOS, Android, as well as UWP/Windows 10."
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
---

# Desktop app porting guidance

Most application code can be categorized into one of the following areas:

- User interface code (eg. windows and buttons)
- 3rd party controls (eg. charts)
- Business logic (eg. validation rules)
- Local data storage and access
- Web services and remote data access

For Windows Forms and WPF applications written with C# (or Visual Basic.NET)
a surprising amount of the business logic, local data access, and web services
code can be shared across platforms.

## .NET Portability Analyzer

Visual Studio 2017 and later support the [.NET Portability Analyzer](/dotnet/articles/standard/portability-analyzer) ([download for Windows](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)) which can examine your existing applications and tell you how much code can be ported "as is" to other platforms. You can learn more about it from this [Channel 9 video](https://channel9.msdn.com/Blogs/Seth-Juarez/A-Brief-Look-at-the-NET-Portability-Analyzer).

There is also a command-line tool can be downloaded from [Portability Analyzer on GitHub](https://github.com/Microsoft/dotnet-apiport) and used to provide the same reports.

## "x% of my code is portable. What next?"

Hopefully the analyzer shows a large portion of your code is portable, but there's certainly going to be some parts of every app that _cannot_ be moved to other platforms.

Different chunks of code will probably fall into one of these buckets, explained in more detail below:

- Re-useable portable code
- Code that requires changes
- Code that isn't portable and requires a re-write

### Re-useable portable code

.NET code that is written against APIs available on all platforms can be
taken cross-platform unchanged. Ideally, you'll be able to move all
this code into a Portable Class Library, Shared Library, or .NET Standard
Library and then test it within your existing app.

That shared library can then be added to application projects for
other platforms (such as Android, iOS, macOS).

### Code that requires changes

Some .NET APIs may not be available on all platforms. If these APIs exist in your code,
you'll need to re-write those sections to use cross-platform APIs.

Examples of this include use of Reflection APIs that are available in
.NET 4.6, but are not available on across all platforms.

After you've re-written the code using portable APIs, you should be able
to package that code in a shared library and test it within your existing app.

### Code that isn't portable and requires a re-write

Examples of code that isn't likely to be cross-platform include:

- **User Interface** – Windows Forms or WPF screens cannot be used in
projects on Android or iOS, for example. Your user-interface will need to be
re-written, using this [Controls Comparison](~/cross-platform/desktop/controls/index.md) as a reference.

- **Platform-specific storage** - Code that relies on a platform-specific
technology (such as a local SQL Server Express database). You'll need to
re-write this using a cross-platform alternative (such as SQLite for the database engine).
Some file system operations may also need to be adjusted, since UWP
has slightly different APIs to Android and iOS (eg. some filesystems are case-sensitive and others aren't).

- **3rd party components** – Check whether 3rd party components in your
applications are available on other platforms. Some, such as non-visual
NuGet packages, might be available but others (especially visual controls like charts
or media players)

## Tips for making code portable

- **Dependency Injection** – Provide different implementations for
each platform, and

- **Layered approach** – Whether MVVM, MVC, MVP, or some other pattern that helps
you separate the portable code from the platform-specific code.

- **Messaging** – You can use message passing in your code to de-couple interactions
between different parts of the application.