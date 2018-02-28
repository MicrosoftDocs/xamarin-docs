---
title: "ScrollView"
description: "Use ScrollView to present layouts that can't fit on just one screen and have content make room for the keyboard."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2016
---

# ScrollView

[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) contains layouts and enables them to scroll offscreen. `ScrollView` is also used to allow views to automatically move to the visible portion of the screen when the keyboard is showing.

[ ![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png "Xamarin.Forms Layouts")

This article covers:

- **[Purpose](#Purpose)** &ndash; the purpose for `ScrollView` and when it is used.
- **[Usage](#Usage)** &ndash; how to use `ScrollView` in practice.
- **[Properties](#Properties)** &ndash; public properties that can be read and modified.
- **[Methods](#Methods)** &ndash; public methods that can be called to scroll the view.
- **[Events](#Events)** &ndash; events that can be used to listen to changes in the view's states.

## Purpose

`ScrollView` can be used to ensure that larger views display well on smaller phones. For example, a layout that works on an iPhone 6s may be clipped on an iPhone 4s. Using a `ScrollView` would allow the clipped portions of the layout to be displayed on the smaller screen.

## Usage

> [!NOTE]
> `ScrollView`s should not be nested. In addition, `ScrollView`s should not be nested with other controls that provide scrolling, like `ListView` and `WebView`.

`ScrollView` exposes a `Content` property which can be set to a single view or layout. Consider this example of a layout with a very large boxView, followed by an `Entry`:

```xaml
<ContentPage.Content>
	<ScrollView>
		<StackLayout>
			<BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
			<Entry />
		</StackLayout>
	</ScrollView>
</ContentPage.Content>
```

In C#:

```csharp
var scroll = new ScrollView();
Content = scroll;
var stack = new StackLayout();
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,	HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

Before the user scrolls down, only the `BoxView` is visible:

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

Notice that when the user starts to enter text in the `Entry`, the view scrolls to keep it visible on screen:

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## Properties

ScrollView has the following properties:

- **Content** &ndash; gets or sets the view to display in the `ScrollView`.
- **[ContentSize](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/)** &ndash; read-only, gets the size of the content, which has a width and height component. This is a bindable property
- **[Orientation](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)** &ndash; This is a [`ScrollOrientation`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/), which is an enumeration that can be set to `Horizontal`, `Vertical`, or `Both`.
- **ScrollX** &ndash; read-only, gets the current scroll position in the X dimension.
- **ScrollY** &ndash; read-only, gets the current scroll position in the Y dimension.

## Methods

`ScrollView` provides a `ScrollToAsync` method, which can be used to scroll the view either using coordinates or by specifying a particular view that should be made visible.

When using coordinates, specify the `x` and `y` coordinates, along with a boolean indicating whether the scrolling should be animated:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

When scrolling to a particular element, the `ScrollToPosition` enumeration specifes where in the view the element will appear:

- **Center** &ndash; scrolls the element to the center of the visible portion of the view.
- **End** &ndash; scrolls the element to the end of the visible portion of the view.
- **MakeVisible** &ndash; scrolls the element so that it is visible within the view.
- **Start** &ndash; scrolls the element to the start of the visible portion of the view.

The `IsAnimated` property specifies how the view will be scrolled. When set to true, a smooth animation will be used, rather than instantly moving the content into view.

## Events

`ScrollView` exposes just one event, `Scrolled`. `Scrolled` is raised when the view has finished scrolling. The event handler for `Scrolled` takes `ScrolledEventArgs`, which has the `ScrollX` and `ScrollY` properties. The following demonstrates how to update a label with the current scroll position of a `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
	label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Note that scroll positions may be negative, due to the bounce effect when scrolling at the end of a list.


## Related Links

- [Layout (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble Example (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
