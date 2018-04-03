---
title: "Summary of Chapter 2. Anatomy of an app"
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 8764EB7D-8331-4CF7-9BE1-26D0DEE9E0BB
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
---

# Summary of Chapter 2. Anatomy of an app

In a Xamarin.Forms application, objects that occupy space on the screen are known as *visual elements*, encapsulated by the [`VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) class. Visual Elements can be split into three categories corresponding to these classes:

- [Page](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)
- [Layout](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/)
- [View](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)

A `Page` derivative occupies the entire screen, or nearly the entire screen. Often, the child of a page is a `Layout` derivative to organize child visual elements. The children of the `Layout` can be other `Layout` classes or `View` derivatives (often called *elements*), which are familiar objects such as text, bitmaps, sliders, buttons, list boxes, and so on.

This chapter demonstrates how to create an application by focusing on the [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), which is the `View` derivative that displays text.

## Say hello

With the Xamarin platform installed, you can create a new Xamarin.Forms solution in Visual Studio or Visual Studio for Mac. The [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello) solution uses a Portable Class Library for the common code. It 
demonstrates a Xamarin.Forms solution created in Visual Studio with no modifications. The solution consists of six projects (the last two of which are not created with the current Xamarin.Forms solution templates):

- [**Hello**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello), a Portable Class Library (PCL) shared by the other projects
- [**Hello.Droid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Droid), an application project for Android
- [**Hello.iOS**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.iOS), an application project for iOS
- [**Hello.UWP**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.UWP), an application project for the Universal Windows Platform (Windows 10 and Windows 10 Mobile)
- [**Hello.Windows**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.Windows), an application project for Windows 8.1
- [**Hello.WinPhone**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Hello/Hello/Hello.WinPhone), an application project for Windows Phone 8.1

You can make any of these application projects the startup project, and then build and run the program on a device or simulator.

In many of your Xamarin.Forms programs, you won't be modifying the application projects. These often remain tiny stubs just to start up the program. Most of your focus will be the Portable Class Library common to all the applications.

## Inside the files

The visuals displayed by the **Hello** program are defined in the constructor of the [`App`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello/App.cs) class. `App` derives from the Xamarin.Forms class [`Application`](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/).

The **References** section of the **Hello** PCL project includes the following Xamarin.Forms assemblies:

- **Xamarin.Forms.Core**
- **Xamarin.Forms.Xaml**
- **Xamarin.Forms.Platform**

The **References** sections of the five application projects include additional assemblies that apply to the individual platforms:

- **Xamarin.Forms.Platform.Android**
- **Xamarin.Forms.Platform.iOS**
- **Xamarin.Forms.Platform.UWP**
- **Xamarin.Forms.Platform.WinRT**
- **Xamarin.Forms.Platform.WinRT.Tablet**
- **Xamarin.Forms.Platform.WinRT.Phone**

Each of the application projects contains a call to the static `Forms.Init` method in the `Xamarin.Forms` namespace. This initializes the Xamarin.Forms library. A different version of `Forms.Init` is defined for each platform. The calls to this method can be found in the following classes:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`App` class, `OnLaunched` method](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- Windows 8.1: [`App` class, `OnLaunched` method](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/App.xaml.cs#L65)
- Windows Phone 8.1: [`App` class, `OnLaunched` method](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WinPhone/App.xaml.cs#L67)

In addition, each platform must instantiate the `App` class location in the PCL. This occurs in a call to `LoadApplication` in the following classes:

- iOS: [`AppDelegate`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.iOS/AppDelegate.cs)
- Android: [`MainActivity`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Droid/MainActivity.cs)
- UWP: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.UWP/MainPage.xaml.cs)
- Windows 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.Windows/MainPage.xaml.cs)
- Windows Phone 8.1: [`MainPage`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter02/Hello/Hello/Hello.WindowsPhone/MainPage.xaml.cs)

Otherwise, these application projects are normal "do nothing" programs.

## PCL or SAP?

It's possible to create a Xamarin.Forms solution with the common code in either a Portable Class Library (PCL) or a Shared Asset Project (SAP). To create an SAP solution, select the Shared option in Visual Studio. The  [**HelloSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/HelloSap) solution demonstrates the SAP template with no modifications.

The PCL approach bundles all the common code in a library project referenced by the platform application projects. With the SAP approach, the common code effectively exists in all the platform application projects and is shared among them.

Most Xamarin.Forms developers prefer the PCL approach. In this book, most of the solutions are PCL. Those that use SAP include an **Sap** suffix in the project name.

To support all the Xamarin.Forms platforms, the version of .NET used by the PCL must accommodate the following platforms:

- .NET Framework 4.5
- Windows 8
- Windows Phone 8.1
- Xamarin.Android
- Xamarin.iOS
- Xamarin.IOS (Classic)

This is known as PC Profile 111.

With the SAP approach the code in the shared project can execute different code for the various platforms by using C# preprocessor directives (`#if`, #`elif`, and `#endif`) with these predefined identifiers:

- iOS: `__IOS__`
- Android: `__ANDROID__`
- UWP: `WINDOWS_UWP`
- Windows 8.1: `WINDOWS_APP`
- Windows Phone 8.1: `WINDOWS_PHONE_APP`

In a PCL you can determine what platform you're running on at runtime, as you'll see later in this chapter.

## Labels for text

The [**Greetings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/Greetings) solution demonstrates how to add a new C# file to the **Greetings** project. This file defines a class named `GreetingsPage` that derives from `ContentPage`. In this book, most projects contain a single `ContentPage` derivative whose name is the name of the project with the suffix `Page` appended.

The `GreetingsPage` constructor instantiates a [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) view, which is the Xamarin.Forms view that displays text. The [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) property is set to the text displayed by the `Label`. This program sets the `Label` to the `Content` property of `ContentPage`. The constructor of the `App` class then instantiates `GreetingsPage` and sets it to its `MainPage` property.

The text is displayed in the upper-left corner of the page. On iOS, this means that it overlaps the page's status bar. There are several solutions to this problem:

### Solution 1. Include padding on the page

Set a [`Padding`](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) property on the page. `Padding` is of type [`Thickness`](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/), a structure with four properties:

- [`Left`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Left/)
- [`Top`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Top/)
- [`Right`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Right/)
- [`Bottom`](https://developer.xamarin.com/api/property/Xamarin.Forms.Thickness.Bottom/)

`Padding` defines an area inside a page where content is excluded. This allows the `Label` to avoid overwriting the iOS status bar.

### Solution 2. Include padding just for iOS (SAP only)

Set a 'Padding' property only on iOS using an SAP with a C# preprocessor directive. This is demonstrated in the [**GreetingsSap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter02/GreetingsSap) solution.

### Solution 3. Include padding just for iOS (PCL or SAP)

In the version of Xamarin.Forms used for the book, a `Padding` property specific to iOS in either a PCL or SAP can be selected using the [`Device.OnPlatform`](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) or [`Device.OnPlatform<T>`](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform%7BT%7D/p/T/T/T/) static method. These methods are now deprecated

The `Device.OnPlatform` methods are used to run platform-specific code or to select platform-specific values. Internally, they make use of the [`Device.OS`](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) static read-only property, which returns a member of the [`TargetPlatform`](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetPlatform/) enumeration:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/)
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/)
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) for Windows 8.1, Windows Phone 8.1, and all UWP devices.
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), previously used to identify Windows Phone 8.0 but is now unused
- [`Other`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Other/) is unused

The `Device.OnPlatform` methods, the `Device.OS` property, and the `TargetPlatform` enumeration are all now deprecated. Instead, use the [`Device.RuntimePlatform`](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) property and compare the `string` return value with the following static fields:

- [`iOS`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), the string "iOS" 
- [`Android`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), the string "Android"
- [`UWP`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), the string "UWP", referring to the Windows Runtime Platform
- [`Windows`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Windows/), the string "Windows" for the Windows Runtime (Windows 8.1 and Windows Phone 8.1)
- [`WinPhone`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/), the string "WinPhone" for Windows Phone 8.0 

The [`Device.Idiom`](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.Idiom/) static read-only property is related. This returns a member of the [`TargetIdiom`](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/), which has these members:

- [`Desktop`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Desktop/)
- [`Tablet`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Tablet/)
- [`Phone`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Phone/)
- [`Unsupported`](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetIdiom.Unsupported/) is unused

For iOS and Android, the cutoff between `Tablet` and `Phone` is a portrait width of 600 units. For the Windows platform, `Desktop` indicates a UWP application running under Windows 10, `Tablet` is a Windows 8.1 program, and `Phone` indicates a UWP application running under Windows 10 or a Windows Phone 8.1 application.

## Solution 3a. Set margin on the Label

The [`Margin`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.Margin/) property was introduced too late to be included in the book, but it is also of type `Thickness` and you can set it on the `Label` to define an area outside the view that is included in the calculation of the view's layout.

The `Padding` property is only available on [`Layout`](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/) and [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) derivatives. The `Margin` property is available on all [`View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) derivatives.

## Solution 4. Center the label within the page

You can center the `Label` within the `Page` (or put it in one of eight other places) by setting the [`HorizontalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) and [`VerticalOptions`](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) properties of the `Label` to a value of type [`LayoutOptions`](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutOptions/). The `LayoutOptions` structure defines two properties:

- An [`Alignment`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Alignment/) property of type [`LayoutAlignment`](https://developer.xamarin.com/api/type/Xamarin.Forms.LayoutAlignment/), an enumeration with four members: [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Start/), which means left or top depending on the orientation, [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Center/), [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.End/), which means right or bottom depending on the orientation, and [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutAlignment.Fill/).

- An [`Expands`](https://developer.xamarin.com/api/property/Xamarin.Forms.LayoutOptions.Expands/) property of type `bool`.

Generally these properties are not used directly. Instead, combinations of these two properties are provided by eight static read-only properties of type `LayoutOptions`:

- [`LayoutOptions.Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Start/)
- [`LayoutOptions.Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Center/)
- [`LayoutOptions.End`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.End/)
- [`LayoutOptions.Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.Fill/)
- [`LayoutOptions.StartAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.StartAndExpand/)
- [`LayoutOptions.CenterAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.CenterAndExpand/)
- [`LayoutOptions.EndAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.EndAndExpand/)
- [`LayoutOptions.FillAndExpand`](https://developer.xamarin.com/api/field/Xamarin.Forms.LayoutOptions.FillAndExpand/)

`HorizontalOptions` and `VerticalOptions` are the most important properties in Xamarin.Forms layout, and are discussed in more detail in [**Chapter 4. Scrolling the Stack**](chapter04.md).

Here's the result with the `HorizontalOptions` and `VerticalOptions` properties of `Label` both set to `LayoutOptions.Center`:

[![Triple screenshot of greetings program](images/ch02fg05-small.png "Horizontally and Vertically Centered Label")](images/ch02fg05-large.png#lightbox "Horizontally and Vertically Centered Label")

## Solution 5. Center the text within the Label

You can also center the text (or place it in eight other locations on the page) by setting the [`HorizontalTextAlignment`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.HorizontalTextAlignment/) and [`VerticalTextAlignment`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.VerticalTextAlignment/) properties of `Label` to a member of the [`TextAlignment`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextAlignment/) enumeration:

- [`Start`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Start/), meaning left or top (depending on orientation)
- [`Center`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.Center/)
- [`End`](https://developer.xamarin.com/api/field/Xamarin.Forms.TextAlignment.End/), meaning right or bottom (depending on orientation)

These two properties are defined only by `Label`, whereas the `HorizontalAlignment` and `VerticalAlignment` properties are defined by `View` and inherited by all `View` derivatives. The visual results might seem similar, but they are very different as the next chapter demonstrates.



## Related Links

- [Chapter 2 full text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch03-Apr2016.pdf)
- [Chapter 2 samples](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03)
- [Chapter 2 F# samples](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter03/FS)
- [Getting Started with Xamarin.Forms](~/xamarin-forms/get-started/index.md)
