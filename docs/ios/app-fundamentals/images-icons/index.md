---
title: "Images and Icons in Xamarin.iOS"
description: "This section includes a variety of articles that cover working with images in a Xamarin.iOS app, such as using them as icons, launch screens or including them in controls and providing icons for custom document types."
ms.service: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
no-loc: [Objective-C]
---

# Images and Icons in Xamarin.iOS

_This section includes a variety of articles that cover working with images in a Xamarin.iOS app, such as using them as icons, launch screens or including them in controls and providing icons for custom document types._

There are several ways that image assets are used inside an iOS app. From simply displaying an image as part of an app's UI to, assigning it to a UI control such as a `UIButton` or `UIImageView`, to providing icons and launch screens, Xamarin.iOS makes it easy to add great artwork to an iOS app in the following ways: 

- **Resolution Independent Images** – Use iOS's built-in support for working with images across different device resolutions and types (iPhone, iPad, etc.).
- **Asset Catalog Image Sets** - Use **Asset Catalog Image Sets** to manage and group all version of a given image asset required by an app.
- **Images in Code** – Use the `UIImage` class's methods to load and work with image assets and assign them to UI controls in C# code.
- **Application Icon** - Define the app icon required by every iOS app. This is the icon that the user will tap from the iOS home screen to launch the app. Additionally, this icon is used by Game Center, if applicable.
- **Spotlight Icon** - Define the app's Spotlight icon. Whenever the user enters the name of an app in a Spotlight Search, this icon is displayed.
- **Settings Icon** - Define the app's **Settings** icon. If the user enters the **Settings** app on their iOS device, this icon will be displayed at the end of the Settings list for the app. 
- **Launch Screens** - Define the app's Launch Screen. After the user taps the app icon and before the first view appears, a blank screen will be shown. Fortunately, iOS includes support for displaying an image in place of the blank screen by using a Storyboard. 
- **iTunes Icon** - Provide an iTune icon. If using the Ad-Hoc method of delivering an app (either for corporate users or for beta testing on real devices), the developer also needs to include a 512x512 and a 1024x1024 image that will be used to represent the app in iTunes.
- **Document Icons** - Use an image as an icon for any specific document type that a Xamarin.iOS app supports or creates.

There are several considerations that should be taken into account when creating image assets for an iOS app, as well as several places where those assets will be used. Each of these have an affect on not only how many image assets will be required, but how those assets are created. The following topics cover the types of images assets that will be required, how those assets are included in the application's bundle and how the image assets are consumed to provide the required functionality:

## [Displaying an Image](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

This article covers including an image asset in a Xamarin.iOS app and displaying that image either by using C# code or by assigning it to a control in the iOS Designer.

## [Application Icons](~/ios/app-fundamentals/images-icons/app-icons.md)

This article covers including and managing an image asset in a Xamarin.iOS app to be used as an App Icon.

## [Alternate App Icons](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple has added several enhancements to iOS 10.3 that allow an app to manage its icon:

- `ApplicationIconBadgeNumber` - Gets or sets the badge of the app icon in the Springboard.
- `SupportsAlternateIcons` - If `true` the app has an alternate set of icons.
- `AlternateIconName` - Returns the name of the alternate icon currently selected or `null` if using the primary icon.
- `SetAlternameIconName` - Use this method to switch the app's icon to the given alternate icon.

## [Launch Screens](~/ios/app-fundamentals/images-icons/launch-screens.md)

This article covers using a special type of Storyboard to provide a universal Launch Screen for every iOS device size and resolution.

## [Custom Document Types](~/ios/app-fundamentals/images-icons/custom-document-types.md)

This article covers including and managing an image asset in a Xamarin.iOS app to be used as a Custom Document Type Icon.

## Related Links

- [Working with Images (sample)](/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Custom Icon and Image Creation Guidelines](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)