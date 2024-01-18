---
title: "Missing packages error after updating NuGet packages"
ms.topic: troubleshooting
ms.service: xamarin
ms.assetid: D61CC966-1D4A-49A5-8A6F-41572E28329B
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
no-loc: [Objective-C]
description: Learn how to fix the missing packages error that occurs after updating NuGet packages by deleting elements that reference the old version number.
---

# Missing packages error after updating NuGet packages

This issue has mainly been reported on Xamarin.Forms sample app solutions, but the potential for this issue can happen on any project that uses NuGet packages.

If after updating NuGet packages in your project or solution, you see an error that references the old package version numbers, such as:

```csharp
Error: This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.
For more information, see http://go.microsoft.com/fwlink/?LinkID=322105

The missing file is ../../packages/Xamarin.Forms.1.3.1.6296/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10/Xamarin.Forms.targets. (FormsGallery)
```

In this example *Xamarin.Forms.1.3.1.6296* is the old version number that was removed with the NuGet package update.

This can happen if the XML elements in the .csproj file that reference the old package version number had been manually added or edited, NuGet will not remove or update them if they had been manually added/edited, so the project is now looking for packages that have been deleted.

To fix this issue,  manually edit the .csproj file(s) and delete all of the elements that reference the old version number.

Sample elements to remove (if they have the old package version number):

```xml
<Reference Include="Xamarin.Forms.Maps">
    <HintPath>..\..\packages\Xamarin.Forms.Maps.1.3.1.6296\lib\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.Maps.dll</HintPath>
</Reference>

<Import Project="..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets" Condition="Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" />
<Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.3.1.6296\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10+Xamarin.iOS10\Xamarin.Forms.targets'))" />
```
