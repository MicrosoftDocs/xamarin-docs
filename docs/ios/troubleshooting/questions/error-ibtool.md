---
title: "IBTool Error: The operation couldn’t be completed."
ms.topic: troubleshooting
ms.service: xamarin
ms.assetid: A804EBC4-2BBF-4A98-A4E8-A455DB2E8A17
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
no-loc: [Objective-C]
description: Learn how to recognize and work around the IBTool Error by managing properties in code, moving images to the top resources folder, and setting the LogicalName.
---

# IBTool Error: The operation couldn’t be completed.

## Fixed in Xcode 6.1.1

Apple [fixed](https://developer.apple.com/library/content/documentation/Xcode/Conceptual/RN-Xcode-Archive/Chapters/xc6_release_notes.html#//apple_ref/doc/uid/TP40016994-CH4-SW1) this `ibtool` bug in Xcode 6.1.1, so upgrading to Xcode 6.1.1 or higher is the easiest fix.

* * *

## Description of the problem

The `ibtool` command in Xcode 6.0 had a bug on OS X 10.10 Yosemite. Xamarin.iOS uses Xcode's `ibtool` to compile storyboards and `XIB` files.

More information about the bug in Xcode can be found on the following Stack Overflow post: [https://stackoverflow.com/questions/25754763/cant-open-storyboard](https://stackoverflow.com/questions/25754763/cant-open-storyboard)

### Error message

> The document "MainStoryboard.storyboard" could not be opened. The operation couldn’t be completed. (com.apple.InterfaceBuilder error -1.)

## Workarounds (for Xcode 6.0)

### Option 1: Manage all `UIImageView.Image` properties in code

Rather than setting the `Image` property of a `UIImageView` in the storyboard or `.xib` file, you can set the property in one of the view's lifecycle override methods in the view controller (for example, in `ViewDidLoad()`). See also [Working with Images](~/ios/app-fundamentals/images-icons/index.md) for tips about using `UIImage.FromBundle()` vs. `UIImage.FromFile()`.

### Option 2: Move all of the image resources to the top level `Resources` folder

After moving the images to the top level `Resources` folder, you will need to update the storyboard and `.xib` files to use the new image paths.

### Option 3: Set the `LogicalName` for any problematic image assets so they are copied to the top level of the`.app` bundle

For example, say your original `.csproj` file contains the following entry:

`<BundleResource Include="Resources\Images\image.png" />`

You can change this element and add a `LogicalName` so that the image will instead be copied to the top level of the `.app` bundle:

```xml
<BundleResource Include="Resources\Images\image.png">
    <LogicalName>image.png</LogicalName>
</BundleResource>
```

In Visual Studio for Mac, the `LogicalName` can also be set using the `Resource ID` field for the image under **View > Pads > Properties**. (See also: [https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545](https://stackoverflow.com/questions/16938250/xamarin-studio-folder-structure-issue-in-ios-project/16951545#16951545))

After this change, you will need to update the storyboard and `.xib` files to use the new top level image paths. In Visual Studio, and Visual Studio for Mac, you'll need to edit the path for the `Image` property by hand.

### Next Steps

For further assistance, to contact us, or if this issue remains even after utilizing the above information, please see [What support options are available for Xamarin?](~/cross-platform/troubleshooting/support-options.md) for information on contact options, suggestions, as well as how to file a new bug if needed. 
