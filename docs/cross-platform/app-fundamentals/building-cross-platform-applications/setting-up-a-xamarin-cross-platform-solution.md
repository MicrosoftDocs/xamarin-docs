---
title: "Part 3 - Setting up a Xamarin cross-platform solution"
description: "This document describes how to set up a cross-platform solution in Xamarin. It discusses various code sharing strategies such as shared projects and .NET Standard."
ms.service: xamarin
ms.assetid: 4139A6C2-D477-C563-C1AB-98CCD0D10A93
author: davidortinau
ms.author: daortin
ms.date: 03/27/2017
no-loc: [Objective-C]
---

# Part 3 - Setting up a Xamarin cross-platform solution

Regardless of what platforms are being used, Xamarin projects all use the
same solution file format (the Visual Studio **.sln** file format). Solutions
can be shared across development environments, even when individual projects
cannot be loaded (such as a Windows project in Visual Studio for Mac).

When creating a new cross-platform application, the first step is to create a
blank solution. This section explains what happens next: setting up the projects for
building cross-platform mobile apps.

## Sharing code

Refer to the [Code Sharing Options](~/cross-platform/app-fundamentals/code-sharing.md) document for a detailed description of how to implement code-sharing across platforms.

### .NET Standard

[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)
projects provide an easy way to share code across platforms, producing assemblies
that can be used across Windows, Xamarin platforms (iOS, Android, Mac), and Linux.
This is the recommended way to share code for Xamarin solutions.

### Other options

Historically Xamarin used [Portable Class Libraries (PCLs)](~/cross-platform/app-fundamentals/pcl.md)] and 
[Shared Projects](~/cross-platform/app-fundamentals/shared-projects.md). Neither of these are recommended for 
new projects; and you should consider migrating existing apps to use .NET Standard.

## Populating the solution

Regardless of which method is used to share code, the overall solution
structure should implement a layered architecture that encourages code sharing.
The Xamarin approach is to group code into two project types:

- **Core (or "Shared") project** – Write re-usable code in one place, to be shared across different platforms. Use the principles of encapsulation to hide implementation details wherever possible.
- **Platform-specific application projects** – Consume the re-usable code with as little coupling as possible. Platform-specific features are added at this level, built on components exposed in the Core project.

### Core project

Core projects that share code should be .NET Standard and only reference assemblies that are available
across all platforms – ie. the common framework namespaces like `System`, `System.Core` and `System.Xml`.

Core projects should implement as much non-UI functionality as is possible,
which could include the following layers:

- **Data Layer** – Code that takes care of physical data storage eg. [SQLite-NET](https://www.nuget.org/packages/sqlite-net-pcl/) or even XML files. The data layer classes are normally only used by the data access layer.
- **Data Access Layer** – Defines an API that supports the required data operations for the application’s functionality, such as methods to access lists of data, individual data items and also create, edit, and delete them.
- **Service Access Layer** – An optional layer to provide cloud services to the application. Contains code that accesses remote network resources (web services, image downloads, etc) and possibly caching of the results.
- **Business Layer** – Definition of the Model classes and the Façade or Manager classes that expose functionality to the platform-specific applications.

### Platform-specific application projects

Platform-specific projects must reference the assemblies required to bind to
each platform’s SDK (Xamarin.iOS, Xamarin.Android, Xamarin.Mac, or Windows) as well as
the .NET Standard project.

The platform-specific projects should implement:

- **Application Layer** – Platform specific functionality and binding/conversion between the Business Layer objects and the user interface.
- **User Interface Layer** – Screens, custom user-interface controls, presentation of validation logic.

## Project references

Project references reflect the dependencies for a project. Core projects
limit their references to common assemblies so that the code is easy to share.
Platform-specific application projects reference the .NET Standard project, plus any other
platform-specific assemblies they need to take advantage of the target
platform.

Specific examples of how projects should be structured are given in the case
studies.

## Adding files

### Build Action

It is important to set the correct build-action for certain file types. This
list shows the build action for some common file types:

- **All C# files** – Build Action: Compile
- **Images in Xamarin.iOS & Windows** – Build Action: Content
- **XIB and Storyboard files in Xamarin.iOS** – Build Action: InterfaceDefinition
- **Images and XML layouts in Android** – Build Action: AndroidResource
- **XAML files in Windows projects** – Build Action: Page
- **Xamarin.Forms XAML files** – Build Action: EmbeddedResource

Generally the IDE will detect the file type and suggest the correct build
action.

### Case sensitivity

Finally, remember that some platforms have case-sensitive file systems (eg.
iOS and Android) so be sure to use a consistent file naming standard and make
sure that the file names you use in code match the filesystem exactly. This is
especially important for images and other resources that you reference in code.
