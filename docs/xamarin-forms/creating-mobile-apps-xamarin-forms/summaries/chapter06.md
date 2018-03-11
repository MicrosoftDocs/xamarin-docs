---
title: "Summary of Chapter 6. Button clicks"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: D4F9C429-A6CF-40FA-AC68-3F149307A5F9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
---

# Summary of Chapter 6. Button clicks

The [`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) is the view that allows the user to initiate a command. A `Button` is identified by text (and optionally an image as demonstrated in [Chapter 13, Bitmaps](chapter13.md)). Consequently, `Button` defines many of the same properties as `Label`:

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Text/)
- [`FontFamily`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontFamily/)
- [`FontSize`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontSize/)
- [`FontAttributes`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.FontAttributes/)
- [`TextColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/)

`Button` also defines three properties that govern the appearance of its border, but the support of these properties and their mutual independence is platform specific:

- [`BorderColor`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderColor/) of type `Color`
- [`BorderWidth`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderWidth/) of type `Double`
- [`BorderRadius`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.BorderRadius/) of type `Double`

`Button` also inherits all the properties of `VisualElement` and `View`, including `BackgroundColor`, `HorizontalOptions`, and `VerticalOptions`.

## Processing the click

The `Button` class defines a [`Clicked`](https://developer.xamarin.com/api/event/Xamarin.Forms.Button.Clicked/) event that is fired when the user taps the `Button`. The `Click` handler is of type `EventHandler`. The first argument is the `Button` object generating the event; the second argument is an `EventArgs` object that provides no additional information.

The [**ButtonLogger**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLogger) sample demonstrates simple `Clicked` handling.

## Sharing button clicks

Multiple `Button` views can share the same `Clicked` handler, but the handler generally needs to determine which `Button` is responsible for a particular event. One approach is to store the various `Button` objects as fields and check which one is firing the event in the handler.

The [**TwoButtons**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/TwoButtons) sample demonstrates this technique. The program also demonstrates how to set the [`IsEnabled`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.IsEnabled/) property of a `Button` to `false` when pressing the `Button` is no longer valid. A disabled `Button` does not generate a `Clicked` event.

## Anonymous event handlers

It is possible to define `Clicked` handlers as anonymous lambda functions, as the [**ButtonLambdas**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/ButtonLambdas) sample demonstrates. However, anonymous handlers can't be shared without some messy reflection code.

## Distinguishing views with IDs

Multiple `Button` objects can also be distinguished by setting the [`StyleId`](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.StyleId/) property or [`AutomationId`](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/) property to a `string`. This property is defined by `Element` but it is not used within Xamarin.Forms. It is intended to be used solely by application programs.

The [**SimplestKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/SimplestKeypad) sample uses the same event handler for all 10 number keys on a numeric keypad and distinguishes between them with the `StyleId` property:

[![Triple screenshot of simplest keypad](images/ch06fg04-small.png "Calculator")](images/ch06fg04-large.png#lightbox "Calculator")

## Saving transient data

Many applications need to save data when a program is terminated and to reload that data when the program starts up again. The [`Application`](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) class defines several members that help your program save and restore transient data:

- The [`Properties`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) property is a dictionary with `string` keys and `object` items. The contents of the dictionary are automatically saved in application local storage prior to program termination, and reloaded when the program starts up.
- The `Application` class defines three protected virtual methods that the program's standard `App` class overrides: [`OnStart`](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnStart()/), [`OnSleep`](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnSleep()/), and [`OnResume`](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnResume()/). These refer to *application lifecycle* events.
- The [`SavePropertiesAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) method saves the contents of the dictionary.

It is not necessary to call `SavePropertiesAsync`. The contents of the dictionary are automatically saved prior to program termination and retrieved before program startup. It's useful during program testing to save data if the program crashes.

Also useful is:

- [`Application.Current`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Current/), a static property that returns the current `Application` object that you can then use to obtain the `Properties` dictionary.

The first step is to identify all the variables on the page that you want to persist when the program terminates. If you know all the places where those variables change, you can simply add them to the `Properties` dictionary at that point. In the page's constructor, you can set the variables from the `Properties` dictionary if the key exists.

A larger program will probably need to deal with application lifecycle events. The most important is the `OnSleep` method. A call to this method indicates that the program has left the foreground. Perhaps the user has pressed the **Home** button on the device, or displayed all the applications, or is shutting down the phone. A call to `OnSleep` is the only notification that a program receives before it is terminated. The program should take this opportunity to ensure that the `Properties` dictionary is up to date.

A call to `OnResume` indicates that the program did not terminate following the last call to `OnSleep` but is now running in the foreground again. The program might use this opportunity to refresh internet connections (for example).

A call to `OnStart` occurs during program startup. It is not necessary to wait until this method call to access the `Properties` dictionary because the contents have already been restored when the `App` constructor is called.

The [**PersistentKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/PersistentKeypad) sample is very similar to **SimplestKeypad** except that the program uses the `OnSleep` override to save the current keypad entry, and the page constructor to restore that data.



## Related Links

- [Chapter 6 full text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch06-Apr2016.pdf)
- [Chapter 6 samples](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06)
- [Chapter 6 F# samples](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter06/FS)
