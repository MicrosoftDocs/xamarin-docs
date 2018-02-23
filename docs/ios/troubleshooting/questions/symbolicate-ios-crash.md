---
title: "Where can I find the .dSYM file to symbolicate iOS crash logs?"
ms.topic: article
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
---

# Where can I find the .dSYM file to symbolicate iOS crash logs?

When building iOS apps from visual studio, the .dSYM file that can be used to symbolicate crash reports ends up on the build host at path:
```
    /Users/<username>/Library/Caches/Xamarin/mtbs/builds/<appname>/<guid>/bin/iPhone/<configuration>
```

Note that the `~/Library` folder is hidden by default in Finder, so if need be use Finder's **Go > Go to Folder** menu and enter: `~/Library/Caches/Xamarin/mtbs/builds/` to open the folder.  

Alternately you can unhide the `~/Library` folder by using the **Show View Options** panel for your home folder. If you select your home folder in the sidebar in Finder and use the Finder menu **View > Show View Options** (or cmd-j), then you will see a checkbox to **Show Library Folder**.


### See Also
- Extended steps for symbolicating iOS crash reports: [http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/](http://jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Demystifying iOS Application Crash Logs](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
