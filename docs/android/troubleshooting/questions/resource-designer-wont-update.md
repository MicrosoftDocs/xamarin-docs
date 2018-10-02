---
title: "My Android Resource.designer.cs file will not update"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/19/2017
---

# My Android Resource.designer.cs file will not update

> [!NOTE]
> This issue has been resolved in Xamarin Studio 5.1.4 and later versions. However, if the issue occurs in Visual Studio for Mac, please file a [new bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md) with your full versioning information and full build log output.

A bug in Xamarin.Studio 5.1 previously corrupted .csproj files by partially or completely deleting the xml code in the .csproj file. This would cause important parts of the Android build system (such as updating the Android Resource.designer.cs) to fail. As of the 5.1.4 stable release on July 15th, this bug has been fixed; but in many cases the project file has to be repaired manually, as described below.


## Two possible approaches to fixing up the project file

### Either:

1) Create a brand new Xamarin.Android application project, set all the project properties to match your old project, and add all of your resources, source files, etc. back into the project.

### OR

2) Make a backup copy of your original project's .csproj file, then open it in a text editor, and add back in the missing elements from a cleanly generated .csproj file.

### If this does not solve the problem

After experimenting with these elements, you may notice that after adding back the elements and rebuilding the project, the Resource.designer.cs file would update, but then you might still have to close and re-open the solution to get code completion to recognize the new types contained in Resource.designer.cs. 
