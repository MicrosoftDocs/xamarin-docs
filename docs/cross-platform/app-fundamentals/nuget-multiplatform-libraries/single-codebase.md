---
title: "Creating a New Multiplatform Library for NuGet"
description: "This document describes how to create a multiplatform library for use with NuGet. This technique is suited for business logic and algorithms that can be expressed entirely in the .NET Base Class library and will thus run on all target platforms without platform-specific code."
ms.prod: xamarin
ms.assetid: E7B55354-9BBE-4122-BCE3-3506B79090DD
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
---

# Creating a New Multiplatform Library for NuGet

Creating a Multiplatform Library project that uses PCL or .NET Standard means that
the resulting NuGet can be added to any .NET project that supports the target profile,
including ASP.NET projects, or desktop apps using WinForms, WPF, or UWP.

The library can only contain code supported by the selected PCL or .NET Standard profile,
as well as any other NuGets that are added.
This is suited to business logic and algorithms that can be expressed entirely in the .NET base class library.

A single assembly is created and built into a NuGet package.

If you later need platform-specific functionality, [platform-specific projects can be added](#add-platforms).

## Steps to Create a Multiplatform Library NuGet

1. Select **File > New Solution** (or right click an existing solution and choose **Add > New Project**).

2. Choose **Multiplatform Library** from the **Multiplatform > Library** section:

   [![Screenshot shows Choose a template with Multiplatform Library selected.](single-codebase-images/mulitplatform-library-sml.png)](single-codebase-images/mulitplatform-library.png#lightbox)

3. Enter a **Name** and **Description**, and choose **Single for all platforms**:

   [![Screenshot shows values entered for Name, Description, and Implementation.](single-codebase-images/single-configure-sml.png)](single-codebase-images/single-configure.png#lightbox)

4. Complete the wizard. A single library project is created in the solution.

5. Right-click on the new library project and then select **Options**. The **Build > General** section
   allows the **Target Framework** to be set – choose a .NET Portable PCL profile or a .NET Standard version:

   [![Choose PCL or .NET Standard for library type](single-codebase-images/single-choose-type-sml.png)](single-codebase-images/single-choose-type.png#lightbox)

6. Also in the **Project Options** window, open the **NuGet Package > Metadata** section and enter the [required metadata](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
   (as well as any optional metadata):

   [![Enter required metadata](single-codebase-images/single-metadata-sml.png)](single-codebase-images/single-metadata.png#lightbox)

7. Right-click on the library project and choose **Create NuGet Package** (or build or deploy the solution) and
   the **.nupkg** NuGet package file will be saved in the **/bin/** folder (either Debug or Release, depending on configuration):

   ![The NuGet package file will be saved in the bin folder either Debug or Release, depending on configuration](single-codebase-images/create-nuget-package.png)

## Verifying the Output

NuGet packages are also ZIP files, so it's possible to inspect the internal structure of the generated package.

This screenshot shows the contents of a PCL-based NuGet – only a single PCL assembly is included:

![Files contained in the NuGet package](single-codebase-images/nuget-output.png)

<a name="add-platforms"></a>

## Adding Platform-Specific Code

PCL-based projects and .NET Standard-based projects cannot contain platform-specific
references (such as iOS or Android functionality).

If an existing PCL project or .NET Standard project needs to be expanded to include platform-specific
code, this can be done by right-clicking on the project and selecting **Add > Add Platform Implementation...**:

[![Add platform implementation menu](single-codebase-images/add-later-sml.png)](single-codebase-images/add-later.png#lightbox)

One or more platform projects can be added to the solution, and the existing PCL or .NET Standard
library can optionally be converted to a Shared Project:

[![Add platform options such as iOS, Android, and Shared Project](single-codebase-images/add-later-platforms-sml.png)](single-codebase-images/add-later-platforms-sml.png#lightbox)

After converting to a Shared Project, visit the **Project Options > NuGet Package > Reference Assemblies**
[section](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/platform-specific.md)
and ensure that any required profiles are selected (so that the NuGet continues to be compatible
with projects it was previously used in).

## Related Links

- [Metadata Guide](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/metadata.md)
