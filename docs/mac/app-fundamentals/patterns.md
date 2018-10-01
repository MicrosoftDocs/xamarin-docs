---
title: "Common patterns and idioms in Xamarin.Mac"
description: "This document describes common design patterns used when building Xamarin.Mac apps. It discusses the model-view-controller pattern, the data source and delegate patterns, and protocols."
ms.prod: xamarin
ms.assetid: BF0A3517-17D8-453D-87F7-C8A34BEA8FF5
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 06/17/2016
---

# Common patterns and idioms in Xamarin.Mac

Throughout the Apple APIs exposed via C#, certain idioms and patterns come up over and over again. If you have experience with programming with Xamarin.iOS, these may look familiar. Documentation will often refer to these patterns and idioms repeatedly, so having a solid understanding of them will help you make sense of the documentation you find.

## MVC - Model View Controller

Model View Controller, or MVC for short, is a very common pattern found throughout Cocoa. A detailed discussion is beyond the scope of this document, but in brief it is a way of structuring your application into components:

- **Model** objects represent the underlying data being viewed and manipulated (Like addresses in an address book)
- **View** objects handle the drawing of a given object on screen and handling user interaction (A text field showing the address on screen)
- **Controller** objects handle the interaction between the Model and View. They push Model changes "up" to update the View and push "down" changes from the View when users make changes in the UI.

If you are familiar with MVVM (Model View ViewModel) from other libraries such as WPF, the Controller acts similar to the ViewModel but is often more closely bound to the specific UI elements.

More details can be found here:

- [Learning MVC on Apple's website](https://developer.apple.com/library/ios/documentation/general/conceptual/devpedia-cocoacore/MVC.html)

- [Model View Controller in Objective-C](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
- [Working with Windows](~/mac/user-interface/window.md)

## Data source / delegate / subclassing

Another very common pattern in Cocoa deals with providing data to UI elements and reacting to user interactions. Using `NSTableView` as an example, you need to somehow provide the data for each row, some set of UI elements that represent that row, some set of behaviors to react to user interactions, and possibly some amount of customization. The data source and delegate patterns let you handle most cases without having to resort to subclassing `NSTableView` yourself.

- The `DataSource` property is assigned an instance of a custom subclass of `NSTableViewDataSource` which is called to populate the table with data (via `GetRowCount` and `GetObjectValue`).

- The `Delegate` property is assigned an instance of a custom subclass of `NSTableViewDelegate` which provides the view for a given model object (via `GetViewForItem`) and handles UI interactions (via `DidClickTableColumn`, `MouseDownInHeaderOfTableColumn`, etc).

In some cases, you’ll want to customize a control in a way beyond the hooks given in the delegate or data source and you can subclass the view directly. Be careful however, in many cases overriding default behavior will then require you to handle all of that behavior yourself (customizing selection behavior may require you to implement all of the selection behaviors yourself).

In Xamarin.iOS, some APIs, such as `UITableView` have been extended with a property that implements both the delegate and the data source (`UITableViewSource`). This it to work around the common limitation that a single C# class can only have one base class, and our surfacing of protocols is done via base classes.

For more information on working with table VIews in a Xamarin.Mac application, please see our [Table View](~/mac/user-interface/table-view.md) documentation.

## Protocols

Protocols in Objective-C can be compared to interfaces in C#, and in many cases are used in similar situations. For example the `NSTableView` example above, both the delegate and the data source are actually protocols. Xamarin.Mac exposes these as  base classes with virtual methods you can override. The primary difference between C# interfaces and Objective-C protocols is that some methods in a protocol may be optional to implement. You’ll have to look at the documentation and/or definition of an API to determine what is optional.

More information please see our [Delegates, Protocols and Events](~/ios/app-fundamentals/delegates-protocols-and-events.md) documentation.



## Related Links

- [Table views](~/mac/user-interface/table-view.md)
- [Working with windows](~/mac/user-interface/window.md)
- [Delegates, protocols, and events](~/ios/app-fundamentals/delegates-protocols-and-events.md)
- [Model-View-Controller](https://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)
