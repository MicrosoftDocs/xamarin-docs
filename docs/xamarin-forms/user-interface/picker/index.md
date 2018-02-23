---
title: "Picker"
description: "The Picker view is a control for selecting a text item from a list of data."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4815A4B-104B-4294-951B-BD8F2EC33C86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/11/2017
---

# Picker

_The Picker view is a control for selecting a text item from a list of data._

A [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) displays a short list of items, from which the user can select. However, a [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) doesn't show any data when it's first displayed. Instead, the value of its [`Title`](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Title/) property is shown as a placeholder on the iOS and Android platforms:

[![](images/picker-initial.png "Initial Picker Display")](images/picker-initial-large.png "Initial Picker Display")

When the [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) gains focus, its data is displayed and the user can select an item:

[![](images/picker-selection.png "Picker Selecting an Item")](images/picker-selection-large.png "Picker Selecting an Item")

Following selection, the selected item is displayed by the [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/):

![](images/picker-after-selection.png "Picker after Selection")

There are two techniques for populating a [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) with data:

- Setting the [`ItemsSource`](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.ItemsSource/) property to the data to be displayed. This is the recommended technique, which was introduced in Xamarin.Forms 2.3.4. For more information, see [Setting a Picker's ItemsSource Property](populating-itemssource.md).
- Adding the data to be displayed to the [`Items`](https://developer.xamarin.com/api/property/Xamarin.Forms.Picker.Items/) collection. This technique was the original process for populating a [`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/) with data. For more information, see [Adding Data to a Picker's Items Collection](populating-items.md).


## Related Links

- [Picker](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)
