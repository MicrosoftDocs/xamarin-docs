---
title: "Working with watchOS Settings in Xamarin"
description: "This document describes how to work with watchOS settings in Xamarin. It discusses adding settings to a watch app solution, using those settings in the app, and the Apple Watch app on the iPhone."
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
---

# Working with watchOS Settings in Xamarin

Apple Watch apps can use the same Settings functionality
  as iOS apps - the settings user interface is displayed
  in the **Apple Watch** iPhone app but the values are accessible
  in both your iPhone app and also the watch extension.

![Apple Watch apps can use the same Settings functionality as iOS apps](settings-images/intro.png)

The settings will be stored in a shared file location that is
  accessible to both the iOS app and the watch app extension,
  defined by an **App Group**. You
  should [configure an App Group](~/ios/watchos/app-fundamentals/app-groups.md)
  before adding the settings using the instructions below.

## Add Settings in a Watch Solution

In the **iPhone app** in your solution (*not* the watch app or extension):

1. Right-click **Add > New File...** and choose **Settings.bundle**
  (you cannot edit the name in the **New File** dialog):

   [![Add a new Settings Bundle](settings-images/settings-add-sml.png)](settings-images/settings-add.png#lightbox)

2. Change the name to **Settings-Watch.bundle**
  (select and type **Command + R** to rename):

   ![Rename the bundle](settings-images/settings-rename.png)

3. Add a new key `ApplicationGroupContainerIdentifier` to the
  **Root.plist** with the value set to the app group you've
  configured, (eg. `group.com.xamarin.WatchSettings` in the sample):

   [![Add a ApplicationGroupContainerIdentifier key to the Root.plist](settings-images/settings-appgroup-sml.png)](settings-images/settings-appgroup.png#lightbox)

4. Edit the **Settings-Watch.bundle/Root.plist** to contain the
  options you wish to use - the template file contains a group.
  textfield, toggle switch and slider by default (which you can
  delete and replace with your own settings):

  [![Edit the Settings-Watch.bundle/Root.plist](settings-images/rootplist-sml.png)](settings-images/rootplist.png#lightbox)

## Use Settings in the Watch App

To access the values selected by the user, create an `NSUserDefaults`
  instance using the app group and specifying `NSUserDefaultsType.SuiteName`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## Apple Watch App

[![The new Apple Watch app on the iPhone](settings-images/settings-app-sml.png)](settings-images/settings-app.png#lightbox)

Users will interact with the settings via the new **Apple Watch**
  app on their iPhone. This app allows the user to show/hide
  apps on the watch, and also edit the settings exposed
  using the **Settings-Watch.bundle**.

![Screenshot shows WatchKitSettings in the app.](settings-images/applewatch-1.png) ![Screenshot shows WatchTodo in the app.](settings-images/applewatch-2.png)

## Related Links

- [Apple's Settings doc](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
