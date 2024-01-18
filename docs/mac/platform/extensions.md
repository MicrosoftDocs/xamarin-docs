---
title: "Xamarin.Mac Extension Support"
description: "This document describes Xamarin.Mac's support for Finder, Share, and Today extensions. It examines limitations and known issues, links to a walkthrough and sample app, and provides tips for working with extensions."
ms.service: xamarin
ms.assetid: 4148F1BE-DFA0-46B6-9FCD-425A6541F510
ms.subservice: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
no-loc: [Objective-C]
---

# Xamarin.Mac Extension Support

In Xamarin.Mac 2.10 support was added for multiple macOS extension points:

- Finder
- Share
- Today

<a name="Limitations-and-Known-Issues"></a>

## Limitations and Known Issues

The following are the limitations and know issues that can occur when developing extensions in Xamarin.Mac:

- There is currently no debugging support in Visual Studio for Mac. All debugging will need to be done via **NSLog** and the **Console**. See the tips section below for details.
- Extensions must be contained in a host application, which when run one time with register with the system. They must then be enabled in the **Extension** section of **System Preferences**. 
- Some extension crashes may destabilize the host application and cause strange behavior. In particular, **Finder** and the **Today** section of the **Notification Center** may become “jammed” and become unresponsive. This has been experienced in extension projects in Xcode as well, and currently appears unrelated to Xamarin.Mac. Often this can be seen in the system log (via **Console**, see Tips for details) printing repeated error messages. Restarting macOS appears to fix this.

<a name="Tips"></a>

## Tips

The following tips can be helpful when working with extensions in Xamarin.Mac:

- As Xamarin.Mac currently does not support debugging extensions, the debugging experience will primarily depend on execution and `printf` like statements. However, extensions run in a sandbox process, thus `Console.WriteLine` will not act as it does in other Xamarin.Mac applications. Invoking [`NSLog` directly](https://gist.github.com/chamons/e2e409013a449cfbe1f2fbe5547f6554) will output debugging messages to the System Log.
- Any uncaught exceptions will crash the extension process, providing only a small amount of useful information in the **System Log**. Wrapping troublesome code in a `try/catch` (Exception) block that `NSLog`’s before re-throwing may be useful.
- The **System Log** can be accessed from the **Console** app under **Applications** > **Utilities**:

    [![The system log](extensions-images/extension02.png)](extensions-images/extension02.png#lightbox)
- As noted above, running the extension host application will register it with the system. Deleting the application bundle with unregister it. 
- If “stray” versions of an app's extensions are registered, use the following command to locate them (so they can be deleted): `plugin kit -mv`

<a name="Walkthrough-and-Sample-App"></a>

## Walkthrough and Sample App

Since the developer will create and work with Xamarin.Mac extensions in the same way as Xamarin.iOS extensions, please refer to our [Introduction to Extensions](~/ios/platform/extensions.md) documentation for more details.

An example Xamarin.Mac project containing small, working samples of each extension type can be found [here](/samples/xamarin/mac-samples/extensionsamples).

<a name="Summary"></a>

## Summary

This article has taken a quick look at working with extensions in a Xamarin.Mac version 2.10 (and greater) app.

## Related Links

- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [ExtensionSamples](/samples/xamarin/mac-samples/extensionsamples)
- [macOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)