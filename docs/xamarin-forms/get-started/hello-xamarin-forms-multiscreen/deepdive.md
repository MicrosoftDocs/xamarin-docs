---
title: "Xamarin.Forms Multiscreen Deep Dive"
ms.topic: article
ms.prod: xamarin
ms.assetid: e4faa36c-6600-48c0-94c4-b4431103a4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/02/2016
---

# Xamarin.Forms Multiscreen Deep Dive

In the [Xamarin.Forms Multiscreen Quickstart](~/xamarin-forms/get-started/hello-xamarin-forms-multiscreen/quickstart.md), the Phoneword application was extended to include a second screen that keeps track of the call history for the application. This article reviews what has been built, to develop an understanding of page navigation and data binding in a Xamarin.Forms application.

## Navigation

Xamarin.Forms provides a built-in navigation model that manages the navigation and user-experience of a stack of pages. This model implements a last-in, first-out (LIFO) stack of `Page` objects. To move from one page to another an application will push a new page onto this stack. To return back to the previous page the application will pop the current page from the stack.

Xamarin.Forms has a [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) class that manages the stack of [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objects. The `NavigationPage` class will also add a navigation bar to the top of the page that displays a title and a platform-appropriate <span class="uiitem">Back</span> button that will return to the previous page. The following code example shows how to wrap a `NavigationPage` around the first page in an application:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new MainPage ());
}
```

All [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instances have a [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) property that exposes methods to modify the page stack. These methods should only be invoked if the application includes a [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/). To navigate to the `CallHistoryPage`, it is necessary to invoke the [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync/p/Xamarin.Forms.Page/) method as demonstrated in the code example below:

```csharp
async void OnCallHistory(object sender, EventArgs e)
{
    await Navigation.PushAsync (new CallHistoryPage ());
}
```

This causes the new `CallHistoryPage` object to be pushed onto the Navigation stack. To programmatically return back to the original page, the `CallHistoryPage` object must invoke the [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) method, as demonstrated in the code example below:

```csharp
await Navigation.PopAsync();
```

However, in the Phoneword application this code isn't required as the [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) class adds a navigation bar to the top of the page that includes a platform appropriate <span class="uiitem">Back</span> button that will return to the previous page.

## Data Binding

Data binding is used to simplify how a Xamarin.Forms application displays and interacts with its data. It establishes a connection between the user interface and the underlying application. The [`BindableObject`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/) class contains much of the infrastructure to support data binding.

Data binding defines the relationship between two objects. The *source* object will provide data. The *target* object will consume (and often display) the data from the source object. In the Phoneword application, the binding target is the [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) control that displays phone numbers, while the `PhoneNumbers` collection is the binding source.

The `PhoneNumbers` collection is declared and initialized in the `App` class, as shown in the following code example:

```csharp
public partial class App : Application
{
   public static List<string> PhoneNumbers { get; set; }

   public App ()
   {
     PhoneNumbers = new List<string>();
     ...
   }
   ...
}
```

The [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) instance is declared and initialized in the `CallHistoryPage` class, as shown in the following code example:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
			 xmlns:local="clr-namespace:Phoneword;assembly=Phoneword"
			 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
			 ...>
    ...
	<ContentPage.Content>
       ...
       <ListView ItemsSource="{x:Static local:App.PhoneNumbers}" />
       ...
	</ContentPage.Content>
</ContentPage>
```

In this example, the [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) control will display the `IEnumerable` collection of data that the [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.ItemsView.ItemsSource/) property binds to. The collection of data can be objects of any type, but by default, `ListView` will use the `ToString` method of each item to display that item. The [`x:Static`](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.StaticExtension/) markup extension is used to indicate that the `ItemsSource` property will be bound to the static `PhoneNumbers` property of the `App` class, which can be located in the `local` namespace.

For more information about data binding, see [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md). For more information about XAML markup extensions, see [XAML Markup Extensions](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md).

## Additional Concepts Introduced in Phoneword

The [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) is responsible for displaying a collection of items on the screen. Each item in the `ListView` is contained in a single cell. For more information about using the `ListView` control, see [ListView](~/xamarin-forms/user-interface/listview/index.md).

## Summary

This article has introduced page navigation and data binding in a Xamarin.Forms application, and demonstrated their use in a multi-screen cross-platform application.
