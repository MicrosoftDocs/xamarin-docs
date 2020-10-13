---
title: "Adjusting Java memory parameters for the Android designer"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 62FAF21C-8090-4AF3-9D88-05A4CFCAFFDC
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/02/2018
---

# Adjusting Java memory parameters for the Android designer

The default memory parameters that are used when starting the `java`
process for the Android designer might be incompatible with some system
configurations.

Starting with Xamarin Studio 5.7.2.7 (and later, Visual Studio for Mac)
and Visual Studio Tools for Xamarin 3.9.344, these settings can be
customized on a per-project basis.

## New Android designer properties and corresponding Java options

The following property names correspond to the indicated java
[command-line option](https://docs.oracle.com/javase/7/docs/technotes/tools/windows/java.html)

- **AndroidDesignerJavaRendererMinMemory** -Xms

- **AndroidDesignerJavaRendererMaxMemory** -Xmx

- **AndroidDesignerJavaRendererPermSize** -XX:MaxPermSize

# [Visual Studio](#tab/windows)

1. Open your solution in Visual Studio.

2. Select each Android project one-by-one in the Solution Explorer and
    click [Show All Files](/previous-versions/visualstudio/visual-studio-2008/4afxey9h(v=vs.90))
    twice on each project. You can skip projects that do not contain
    any `.axml` layout files. This step will ensure that each project
    directory contains a `.csproj.user` file.

3. Quit Visual Studio.

4. Locate the `.csproj.user` file for each of the projects from step 2.

5. Edit each `.csproj.user` file in a text editor.

6. Add any or all of the new Android designer memory properties within
    a `<PropertyGroup>` element. You can use an existing
    `<PropertyGroup>` or create a new one. Here's a complete example
    `.csproj.user` file that includes all 3 attributes set to their
    default values:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="12.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
       <PropertyGroup>
         <ProjectView>ProjectFiles</ProjectView>
       </PropertyGroup>
       <PropertyGroup>
         <AndroidDesignerJavaRendererMinMemory>128m</AndroidDesignerJavaRendererMinMemory>
         <AndroidDesignerJavaRendererMaxMemory>750m</AndroidDesignerJavaRendererMaxMemory>
         <AndroidDesignerJavaRendererPermSize>350m</AndroidDesignerJavaRendererPermSize>
       </PropertyGroup>
    </Project>
    ```

7. Save and close all of the updated `.csproj.user` files.

8. Restart Visual Studio and reopen your solution.

# [Visual Studio for Mac](#tab/macos)

1. Open your solution in Visual Studio for Mac to ensure the solution
    directory contains a `.userprefs` file.

2. Quit Visual Studio for Mac.

3. Locate the `.userprefs` file in the solution directory.

4. Edit the `.userprefs` file in a text editor.

5. Locate the existing XML element with the following format. The last
    part of this element name will match the name of your project:
    "AndroidApplication1" in this example:

    ```xml
    <MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >
    ```

6. If the `<MonoDevelop.Ide.ItemProperties.AndroidApplication1 ... >`
    element does not exist, create it anywhere within the enclosing
    `<Properties>` element. Be sure to replace "AndroidApplication1"
    with the name of your project.

7. Add any or all of the new Android designer memory properties as
    attributes on the element. Here's a complete example `.userprefs`
    file that includes all 3 attributes set to their default values:

    ```xml
    <Properties StartupItem="AndroidApplication1\AndroidApplication1.csproj">
      <MonoDevelop.Ide.Workspace ActiveConfiguration="Debug" PreferredExecutionTarget="Android.SelectDevice" />
      <MonoDevelop.Ide.Workbench />
      <MonoDevelop.Ide.DebuggingService.Breakpoints>
        <BreakpointStore />
      </MonoDevelop.Ide.DebuggingService.Breakpoints>
      <MonoDevelop.Ide.DebuggingService.PinnedWatches />
      <MonoDevelop.Ide.ItemProperties.AndroidApplication1 AndroidDesignerJavaRendererMinMemory="128m" AndroidDesignerJavaRendererMaxMemory="750m" AndroidDesignerJavaRendererPermSize="350m" />
    </Properties>
    ```

8. Repeat steps 5-7 for each Android project in the solution that
    contains any `.axml` layout files. (That is, add one
    `<MonoDevelop.Ide.ItemProperties.ProjectName>` element for each
    project.)

9. Save and close the `.userprefs` file.

10. Restart Visual Studio for Mac and reopen your solution.

-----