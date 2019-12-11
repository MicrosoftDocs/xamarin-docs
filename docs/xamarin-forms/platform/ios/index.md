---
title: "iOS platform features in Xamarin.Forms"
description: "Adding iOS-specific functionality to Xamarin.Forms applications."
ms.prod: xamarin
ms.assetid: 634AB62E-68C8-454C-838B-F1CC4E4E21BC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
---

# iOS platform features in Xamarin.Forms

Developing Xamarin.Forms applications for iOS requires Visual Studio. The [requirements page](~/get-started/requirements.md) contains more information about the pre-requisites.

## Platform-specifics

Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects.

The following platform-specific functionality is provided for Xamarin.Forms views, pages, and layouts on iOS:

- Blur support for any [`VisualElement`](xref:Xamarin.Forms.VisualElement). For more information, see [VisualElement Blur on iOS](visualelement-blur.md).
- Disabling legacy color mode on a supported [`VisualElement`](xref:Xamarin.Forms.VisualElement). For more information, see [VisualElement Legacy Color Mode on iOS](legacy-color-mode.md).
- Enabling a drop shadow on a [`VisualElement`](xref:Xamarin.Forms.VisualElement). For more information, see [VisualElement Drop Shadows on iOS](visualelement-drop-shadow.md).

The following platform-specific functionality is provided for Xamarin.Forms views on iOS:

- Setting the [`Cell`](xref:Xamarin.Forms.Cell) background color. For more information, see [Cell Background Color on iOS](cell-background-color.md).
- Ensuring that inputted text fits into an [`Entry`](xref:Xamarin.Forms.Entry) by adjusting the font size. For more information, see [Entry Font Size on iOS](entry-font-size.md).
- Setting the cursor color in a [`Entry`](xref:Xamarin.Forms.Entry). For more information, see [Entry Cursor Color on iOS](entry-cursor-color.md).
- Controlling whether [`ListView`](xref:Xamarin.Forms.ListView) header cells float during scrolling. For more information, see [ListView Group Header Style on iOS](listview-group-header-style.md).
- Controlling whether row animations are disabled when the [`ListView`](xref:Xamarin.Forms.ListView) items collection is being updated. For more information, see [ListView Row Animations on iOS](listview-row-animations.md).
- Setting the separator style on a [`ListView`](xref:Xamarin.Forms.ListView). For more information, see [ListView Separator Style on iOS](listview-separator-style.md).
- Controlling when item selection occurs in a [`Picker`](xref:Xamarin.Forms.Picker). For more information, see [Picker Item Selection on iOS](picker-selection.md).
- Enabling the [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) property to be set by tapping on a position on the [`Slider`](xref:Xamarin.Forms.Slider) bar, rather than by having to drag the `Slider` thumb. For more information, see [Slider Thumb Tap on iOS](slider-thumb.md).
- Controlling the transition that's used when opening a `SwipeView`. For more information, see [SwipeView Swipe Transition Mode](swipeview-swipetransitionmode.md).

The following platform-specific functionality is provided for Xamarin.Forms pages on iOS:

- Hiding the navigation bar separator on a [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). For more information, see [NavigationPage Bar Separator on iOS](navigation-bar-separator.md).
- Controlling whether the navigation bar is translucent. For more information, see [Navigation Bar Translucency on iOS](navigation-bar-translucent.md).
- Controlling whether the status bar text color on a [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) is adjusted to match the luminosity of the navigation bar. For more information, see [NavigationPage Bar Text Color Mode on iOS](status-bar-text-color.md).
- Controlling whether the page title is displayed as a large title in the page navigation bar. For more information, see [Large Page Titles on iOS](page-large-title.md).
- Setting the visibility of the home indicator on a [`Page`](xref:Xamarin.Forms.Page). For more information, see [Home Indicator Visibility on iOS](page-home-indicator.md).
- Setting the status bar visibility on a [`Page`](xref:Xamarin.Forms.Page). For more information, see [Page Status Bar Visibility on iOS](page-status-bar-visibility.md).
- Ensuring that page content is positioned on an area of the screen that is safe for all iOS devices. For more information, see [Safe Area Layout Guide on iOS](page-safe-area-layout.md).
- Setting the presentation style of modal pages. For more information, see [Modal Page Presentation Style](page-presentation-style.md).

The following platform-specific functionality is provided for Xamarin.Forms layouts on iOS:

- Controlling whether a [`ScrollView`](xref:Xamarin.Forms.ScrollView) handles a touch gesture or passes it to its content. For more information, see [ScrollView Content Touches on iOS](scrollview-content-touches.md).

The following platform-specific functionality is provided for the Xamarin.Forms [`Application`](xref:Xamarin.Forms.Application) class on iOS:

- Disabling accessibility scaling for named font sizes. For more information, see [Accessibility Scaling for Named Font Sizes on iOS](named-font-size-scaling.md).
- Enabling control layout and rendering updates to be performed on the main thread. For more information, see [Main Thread Control Updates on iOS](main-thread-updates-ui.md).
- Enabling a [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) in a scrolling view to capture and share the pan gesture with the scrolling view. For more information, see [Simultaneous Pan Gesture Recognition on iOS](application-pan-gesture.md).

## iOS-specific formatting

Xamarin.Forms enables cross-platform user interface styles and colors to be set - but there are other options for setting the theme of your iOS using platform APIs in the iOS project.

[Read more](formatting.md) about formatting the user interface using iOS-specific APIs, such as **Info.plist** configuration and the `UIAppearance` API.

![](images/status-white-sml.png "iOS Theming")

## Other iOS features

Using [custom renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md), the [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md), and the [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md), it's possible to incorporate a wide variety of native functionality into Xamarin.Forms applications for iOS.
