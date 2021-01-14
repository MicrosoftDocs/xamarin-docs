---
title: "Where can I find the .dSYM file to symbolicate iOS crash logs?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/09/2018
---

# Where can I find the .dSYM file to symbolicate iOS crash logs?

When building an iOS app with Visual Studio for Mac or Visual Studio 2017,
the .dSYM file that's needed to symbolicate crash reports will be placed in 
the same directory hierarchy as your app's project file (.csproj). The exact
location depends on your project's build settings:

- If you have enabled device-specific builds, the .dSYM can be found in 
the following directory:

    **&lt;project directory&gt;/bin/&lt;platform&gt;/&lt;configuration&gt;/device-builds/&lt;device&gt;-&lt;os-version&gt;/**

    For example:
  
    **TestApp/bin/iPhone/Release/device-builds/iphone8.4-11.3.1/**

- If you have not enabled device-specific builds, the .dSYM can be found in 
the following directory:

    **&lt;project directory&gt;/bin/&lt;platform&gt;/&lt;configuration&gt;/**

    For example:

    **TestApp/bin/iPhone/Release/**

> [!NOTE]
> As part of the build process, Visual Studio 2017 copies the .dSYM file 
> from the Mac build host to Windows. If you do not see a .dSYM file on 
> Windows, be sure you have configured your app's build settings to
> [create an .ipa file](~/ios/deploy-test/app-distribution/ipa-support.md).

## See also

- [Symbolicating iOS Crash Files (Xamarin.iOS)](https://www.jmillerdev.com/symbolicating-ios-crash-files-xamarin-ios/)
- [Demystifying iOS Application Crash Logs](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)
