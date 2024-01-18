---
title: "watchOS User Interface Controls in Xamarin"
description: "This document describes the various controls that are available for use in watchOS user interfaces. It provides a description of labels, buttons, switches, sliders, images, separators, maps, and more."
ms.service: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/19/2016
no-loc: [Objective-C]
---

# watchOS User Interface Controls in Xamarin

The [**WatchKitCatalog**](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) sample
  demonstrates various watchOS controls. The app's storyboard
  is shown here (click to zoom):

[![Sample watchOS layout](images/storyboard-sml.png)](images/storyboard.png#lightbox)

The programmatic names of all the controls is prefixed with
  `WKInterface` (eg. `WKInterfaceLabel`, `WKInterfaceButton`).

|Control|Description|Screenshot|
|---|---|---|
|Label|Use `SetText` and other properties to control the appearance of text in a label control. `NSAttributedString` is also supported.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![Label screenshot](Images/label.png)|
|Button|Create and set properties in the storyboard. Ctrl+drag to add an `Action` to implement a handler for when it's clicked.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![Button screenshot](Images/button.png)|
|Switch|Use `SetOn` to control the switch state.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![Switch screenshot](Images/switch.png)|
|Slider|Many different styles are possible.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![Slider screenshot](Images/slider.png)|
|Image|Use `myImage.SetImage("MyWatchImage")` to load images on the watch, or `WKInterfaceDevice.CurrentDevice.AddCachedImage` to cache them for repeated use on the watch.<br />[Image Control documentation](~/ios/watchos/user-interface/image.md)<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![Image screenshot](Images/image.png)|
|Separator|Use separators to help create attractive watch UIs.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![Separator screenshot](Images/separator.png)|
|Map|The map image is statically displayed on the watch but you can control many aspects of its appearance, including adding pins.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![Map screenshot](Images/map.png)|
|Movie & InlineMove|Movies can either open on their own, or inline<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![Movie screenshot](Images/movie.png)|
|Group|Use groups to help create attractive watch UIs.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![Group screenshot](Images/group.png)|
|Table|A simplified version of tables on iOS. Implement `DidSelectRow` to respond to user selection (or use a segue).<br />[Table Control documentation](~/ios/watchos/user-interface/table.md)<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![Table screenshot](Images/table.png)|
|Device|`WKInterfaceDevice.CurrentDevice` includes properties such as `ScreenBounds`, `ScreenScale`, and `PreferredContentSizeCategory`.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![Device screenshot](Images/device.png)|
|[Menu](~/ios/watchos/user-interface/menu.md)|Define the force-press menu in the storyboard and implement the actions for each button in the code.<br />[Menu Control (Force Touch) documentation](~/ios/watchos/user-interface/menu.md)<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![Menu screenshot](Images/controller.png)|
|Text Input|Use `PresentTextInputController` and the `WKTextInputMode` enumeration.<br />[Text Input documentation](~/ios/watchos/user-interface/text-input.md)<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![Text input screenshot](Images/textinput.png)|
|Digital Crown|The Digital Crown can be used to drive a picker, or it's rotation can be tracked in code.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![Digital crown screenshot](Images/digital-crown.png)|
|Gestures|There are four types of gesture recognition that can be added to a scene: Tap, Swipe, Pan, and LongPress.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![Gestures screenshot](Images/gestures.png)|

## Related Links

- [WatchKitCatalog (sample)](/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Watch Kit API Reference](xref:WatchKit)