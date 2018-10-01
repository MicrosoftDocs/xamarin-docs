---
title: "Is it possible to create a .xcarchive archive from Visual Studio?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
---

# Is it possible to create a .xcarchive archive from Visual Studio?

## For Xamarin 4

As of Xamarin 4.x, it is now possible to create a `.xcarchive` from Windows by setting the `ArchiveOnBuild` property to `true`. For example, using `MSBuild` on the command line:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

The `.xcarchive` will be placed in the `$HOME/Library/Developer/Xcode/Archives` directory on the Mac build host that both Xcode and Xamarin Studio search to display previously built archives.

See this [Xamarin Forums post](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) for some brief additional notes about the `ArchiveOnBuild` property. See the documentation about [Xamarin.iOS command line builds on Windows](~/ios/get-started/installation/windows/connecting-to-mac/index.md) for additional details about the `ServerAddress` and `ServerUser` properties.

* * *

## For Xamarin 3 and earlier

The Xamarin 3.x Visual Studio extension does not provide a mechanism to produce `.xcarchive` archives. That said, the logic used to create `.xcarchive` archives in Xamarin Studio on Mac [is described here](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5), so you could probably create your own `.xcarchive` by hand if you wished.

But it's worth noting that you don't need a `.xcarchive` to submit to the App Store. You can submit an IPA file as long as it's signed with an App Store Distribution Profile (not an Ad Hoc Distribution Profile).

In fact, you can even just zip up the `.app` bundle (that's signed with an App Store Distribution Profile), and submit that `.zip` file to the app store.

In either case, you can use the Application Loader app to submit the app (rather than Xcode).

