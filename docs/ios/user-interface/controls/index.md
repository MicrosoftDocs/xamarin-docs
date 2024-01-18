---
title: "User Interface Controls in Xamarin.iOS"
description: "This document links to guides that describe the various iOS user interface controls available to Xamarin.iOS developers. Linked content discusses alerts, buttons, collection views, images, manual camera controls, maps, labels, pickers, date pickers, and more."
ms.service: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
no-loc: [Objective-C]
---

# User Interface Controls in Xamarin.iOS

This document introduces some of the most common iOS user interface controls and how to use them.

## [Alerts](alerts.md)

Starting with iOS 8, UIAlertController has completed replaced UIActionSheet and UIAlertView both of which are now deprecated.

## [Buttons](buttons.md)

The UIButton class is used to represent various different styles of button in iOS screens. This section introduces the different options for working with buttons in iOS.

## [Collection Views](uicollectionview.md)

Collection Views, available in the `UICollectionView` class, are a
new concept in iOS 6 that introduce presenting multiple items on the screen using layouts. The patterns for providing data to a `UICollectionView` to create items and interact with those items follow the same delegation and data source patterns commonly used in iOS
development.

## [Images](image.md)

Adding images to your app requires two steps: first, add the images to your project; then, add controls and code to display them on a screen. Refer to the [Working with Images](~/ios/app-fundamentals/images-icons/index.md) article for more detailed coverage of image handling in Xamarin.iOS.

## [Manual Camera Controls](intro-to-manual-camera-controls.md)

The Manual Camera Controls, provided by the `AVFoundation Framework` in iOS 8, allow a mobile application to take full control over an iOS device's camera. This fine-grained level of control can be used to create professional level camera applications and provide artist compositions by tweaking the parameters of the camera while taking a still image or video.

## [Maps](ios-maps/index.md)

Maps are a common feature in all modern mobile operating systems. iOS offers mapping support natively through the Map Kit framework. With Map Kit, applications can easily add rich, interactive maps. These maps can be customized in a variety of ways, such as adding annotations to mark locations on a map, and overlaying graphics of arbitrary shapes. Map Kit even has built-in support for showing the current location of a device.

## [Labels](labels.md)

The `UILabel` control is used for displaying single and multi-line, read only text.

## [Pickers and Date Pickers](picker.md)

The Picker control displays 'wheel-like' control that contains a scrollable list of values with the selected value being highlighted. Users rotate the wheel to select the option they want.

One specific user case for pickers it to set the date and / or time. To provide for this Apple has created a custom subclass of the UIPickerView class called UIDatePicker.

## [Progress and Activity Indicators](progress-activity-indicator.md)

iOS provides two main ways to  indicate progress in your app: Activity Indicators (including a specific _network_ activity indicator) and Progress Bars.

## [Search Bars](searchbar.md)

The UISearchBar is used to search through a list of values. 

## [Sliders, Switches, and Segmented Controls](slider-switch-segmented-controls.md)

The slider control allows for simple selection of a numeric value within a range. iOS uses the `UISwitch` as a boolean input that may be represented by a radio-button on other platforms. A Segmented Control is an organized way to allow users to interact with a small number of options.

## [Stack View](uistackview.md)

The Stack View control (`UIStackView`) leverages the power of Auto Layout and Size Classes to manage a stack of subviews, either horizontally or vertically, which dynamically responds to the orientation and screen size of the iOS device.

## [Tables and Cells](tables/index.md)

This section introduces the classes used to create and display tables then provides examples of how to use them in Xamarin.iOS. It will cover using the default appearance for tables, customizing the layout, implementing editing and using the Xamarin iOS Designer to design a table visually. Sometimes the display is obviously a list of rows (such as the Music app) and other times it is difficult to recognize the table control (such as editing in the Contacts app, or a conversation in Messages app).

## [Text Input](text-input.md)

Accepting user text input is accomplished with the `UITextField` for single-line inputs and UITextView for multi-line editable text. You can drag either of these controls onto a screen and double-click to set the initial text.

## [Tab Bars and Tab Bar Controllers](creating-tabbed-applications.md)

iOS applications using a tab-navigation UI are built using the UITabBarController class. In this article we’ll walk through how to set up a tabbed application that contains several controllers and views. We’ll then examine how to load a UITabBarController when it is not the root controller, such as after a login screen.

## [Web Views](webview.md)

In this article, we will explore the web views provided by Apple–`WKWebview` and `SFSafariViewController`–their similarities and differences, and how they can be used.

## Related Links

- [Controls (sample)](/samples/xamarin/ios-samples/controls)
