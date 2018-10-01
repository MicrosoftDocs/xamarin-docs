---
title: "watchOS User Interface Controls in Xamarin"
description: "This document describes the various controls that are available for use in watchOS user interfaces. It provides a description of labels, buttons, switches, sliders, images, separators, maps, and more."
ms.prod: xamarin
ms.assetid: EDFAD203-02EA-4A74-9CE2-7B8513BC90E1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/19/2016
---

# watchOS User Interface Controls in Xamarin

The [**WatchKitCatalog**](https://github.com/xamarin/monotouch-samples/tree/master/watchOS/WatchKitCatalog) sample
  demonstrates various watchOS controls. The app's storyboard
  is shown here (click to zoom):

[![](images/storyboard-sml.png "Sample watchOS layout")](images/storyboard.png#lightbox)

The programmatic names of all the controls is prefixed with
  `WKInterface` (eg. `WKInterfaceLabel`, `WKInterfaceButton`).

|Control|Description|Screenshot|
|---|---|---|
|Label|Use `SetText` and other properties to control the appearance of text in a label control. `NSAttributedString` is also supported.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/LabelDetailController.cs)|![](Images/label.png)|
|Button|Create and set properties in the storyboard. Ctrl+drag to add an `Action` to implement a handler for when it's clicked.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ButtonDetailController.cs)|![](Images/button.png)|
|Switch|Use `SetOn` to control the switch state.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SwitchDetailController.cs)|![](Images/switch.png)|
|Slider|Many different styles are possible.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SliderDetailController.cs)|![](Images/slider.png)|
|Image|Use `myImage.SetImage("MyWatchImage")` to load images on the watch, or `WKInterfaceDevice.CurrentDevice.AddCachedImage` to cache them for repeated use on the watch.<br />[Image Control documentation](~/ios/watchos/user-interface/image.md)<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ImageDetailController.cs)|![](Images/image.png)|
|Separator|Use separators to help create attractive watch UIs.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/SeparatorDetailController.cs)|![](Images/separator.png)| 
|Map|The map image is statically displayed on the watch but you can control many aspects of its appearance, including adding pins.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MapDetailController.cs)|![](Images/map.png)|
|Movie & InlineMove|Movies can either open on their own, or inline<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/MovieDetailController.cs)|![](Images/movie.png)|
|Group|Use groups to help create attractive watch UIs.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GroupDetailController.cs)|![](Images/group.png)|
|Table|A simplified version of tables on iOS. Implement `DidSelectRow` to respond to user selection (or use a segue).<br />[Table Control documentation](~/ios/watchos/user-interface/table.md)<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/Table%20Detail%20Controller/TableDetailController.cs)|![](Images/table.png)|
|Device|`WKInterfaceDevice.CurrentDevice` includes properties such as `ScreenBounds`, `ScreenScale`, and `PreferredContentSizeCategory`.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/DeviceDetailController.cs)|![](Images/device.png)|
|[Menu](~/ios/watchos/user-interface/menu.md)|Define the force-press menu in the storyboard and implement the actions for each button in the code.<br />[Menu Control (Force Touch) documentation](~/ios/watchos/user-interface/menu.md)<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/ControllerDetailController.cs)|![](Images/controller.png)|
|Text Input|Use `PresentTextInputController` and the `WKTextInputMode` enumeration.<br />[Text Input documentation](~/ios/watchos/user-interface/text-input.md)<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/TextInputController.cs)|![](Images/textinput.png)|
|Digital Crown|The Digital Crown can be used to drive a picker, or it's rotation can be tracked in code.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/CrownDetailController.cs)|![](Images/digital-crown.png)|
|Gestures|There are four types of gesture recognition that can be added to a scene: Tap, Swipe, Pan, and LongPress.<br />[Catalog code](https://github.com/xamarin/ios-samples/blob/master/watchOS/WatchKitCatalog/WatchKit3Extension/GestureDetailController.cs)|![](Images/gestures.png)|


## Related Links

- [WatchKitCatalog (sample)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Watch Kit API Reference](https://developer.xamarin.com/api/namespace/WatchKit/)
