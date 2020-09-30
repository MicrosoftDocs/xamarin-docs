---
title: "How do I resolve a PathTooLongException error?"
description: "This article explains how to resolve a PathTooLongException that may occur while building an app."
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 60EE1C8D-BE44-4612-B3B5-70316D71B1EA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/29/2018
---

# How do I resolve a PathTooLongException error?

## Cause

Generated path names in a Xamarin.Android project can be quite long.
For example, a path like the following could be generated during a
build:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\__library_projects__\\Xamarin.Forms.Platform.Android\\library_project_imports\\assets**

On Windows (where the maximum length for a path is
[260 characters](/windows/win32/fileio/naming-a-file)),
a **PathTooLongException** could be produced while building the
project if a generated path exceeds the maximum length. 

## Fix

The `UseShortFileNames` MSBuild
property is set to `True` to circumvent this error by default. When this property is set
to `True`, the build process uses shorter path
names to reduce the likelihood of producing a **PathTooLongException**.
For example, when `UseShortFileNames` is set to `True`, the above path
is shortened to path that is similar to the following:

**C:\\Some\\Directory\\Solution\\Project\\obj\\Debug\\lp\\1\\jl\\assets**

To set this property manually, add the following MSBuild property to the
project **.csproj** file:

```xml
<PropertyGroup>
    <UseShortFileNames>True</UseShortFileNames>
</PropertyGroup>
```

If setting this flag does not fix the **PathTooLongException** error,
another approach is to specify a
[common intermediate output root](/archive/blogs/kirillosenkov/using-a-common-intermediate-and-output-directory-for-your-solution)
for projects in your solution by setting `IntermediateOutputPath` in
the project **.csproj** file. Try to use a relatively short path. For
example:

```xml
<PropertyGroup>
    <IntermediateOutputPath>C:\Projects\MyApp</IntermediateOutputPath>
</PropertyGroup>
```

For more information about setting build properties, see
[Build Process](~/android/deploy-test/building-apps/build-process.md).