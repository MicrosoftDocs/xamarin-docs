---
title: "Dark Mode in Xamarin.iOS"
description: "Dark Mode is a new system-wide option for light and dark themes. iOS user may now choose a theme or allow iOS to dynamically change appearance."
ms.prod: xamarin
ms.assetid: 74B9973B-C1E8-B727-3F6D-59C1F98BAB3A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/28/2019
---

# Dark Mode in Xamarin.iOS

![](~/media/shared/preview.png "This API is currently in preview")

Dark Mode is a system-wide option for light and dark themes. iOS users may now choose the theme or allow iOS to dynamically change appearance based on the environment and time of day. 

This document introduces dark mode and supporting dark mode in iOS 13 applications.

## Requirements

The feature discussed in this document require iOS 13 and Xcode
11, Xamarin.iOS 12.99, and Visual Studio 2019 or Visual Studio 2019 for Mac with Xcode 11 support.

## Turning on Dark Mode

Apple provides a developer menu in iOS 13 to toggle between dark and light modes. In the iOS 13 simulator open Settings and scroll down to Developer. There you’ll find a switch for turning on or off dark appearance. The change will be reflected across the entire simulator environment. 

<img class="alignleft size-full wp-image-45184" src="http://devblogs.microsoft.com/xamarin/wp-content/uploads/sites/44/2019/08/LightAndDark_DeveloperSetting.png" alt="Turning on Dark Mode" width="782" height="700" />

## Assets for Light and Dark Modes

The Asset Catalog in Visual Studio now supports optional images and colors for each appearance mode: Universal, Dark, and Light. When defining your images and colors this way, iOS will automatically choose the appropriate image and color for you. 

Open your Assets.xcassets file in your iOS project and add a new image set. Notice you can specify universal, dark, and light images at any of the target resolutions. In the example below, there is an image for dark and for light with the name of “MicrosoftLogo”.

<img class="alignleft size-full wp-image-45194" src="http://devblogs.microsoft.com/xamarin/wp-content/uploads/sites/44/2019/08/LightAndDark_AssetCatalog2.png" alt="Assets for Light and Dark Modes img1" width="2624" height="1640" />

Notice there are also colors specified for "BackgroundColor" and "TitleColor". Those colors are now available by name to be used throughout the application. The "BackgroundColor" has been assigned to the background of the view, and the "TitleColor" to the label you see on screen.

<img class="alignleft size-full wp-image-45185" src="http://devblogs.microsoft.com/xamarin/wp-content/uploads/sites/44/2019/08/LightAndDark_01.png" alt="Assets for Light and Dark Modes img2" width="782" height="700" />

## Dynamic System Colors

Apple has introduced new semantic colors that adjust their appearance dynamically based on the new dark mode setting.


## Summary

This article introduced Dark Mode for iOS and specifying images and colors for each mode using the asset catalog.

## Related Links

- [Dark Mode Design Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/dark-mode/)
- [Semantic Colors](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/#dynamic-system-colors)
