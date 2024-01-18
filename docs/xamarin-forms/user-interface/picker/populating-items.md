---
title: "Adding Data to a Picker's Items Collection"
description: "The Picker view is a control for selecting a text item from a list of data. This article explains how to populate a Picker with data by adding it to the Items collection, and how to respond to item selection by the user."
ms.service: xamarin
ms.assetid: 3C840F64-A430-457D-A4B2-3D7AF46F9DBE
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Adding Data to a Picker's Items Collection

<!-- [![Download Sample](~/media/shared/download.png) Download the sample](https://github.com/xamarin/xamarin-forms-samples/tree/main/UserInterface/PickerDemo/PickerDemo) -->

_The Picker view is a control for selecting a text item from a list of data. This article explains how to populate a Picker with data by adding it to the Items collection, and how to respond to item selection by the user._

## Populating a Picker with data

Prior to Xamarin.Forms 2.3.4, the process for populating a [`Picker`](xref:Xamarin.Forms.Picker) with data was to add the data to be displayed to the read-only [`Items`](xref:Xamarin.Forms.Picker.Items) collection, which is of type `IList<string>`. Each item in the collection must be of type `string`. Items can be added in XAML by initializing the `Items` property with a list of `x:String` items:

```xaml
<Picker Title="Select a monkey"
        TitleColor="Red">
  <Picker.Items>
    <x:String>Baboon</x:String>
    <x:String>Capuchin Monkey</x:String>
    <x:String>Blue Monkey</x:String>
    <x:String>Squirrel Monkey</x:String>
    <x:String>Golden Lion Tamarin</x:String>
    <x:String>Howler Monkey</x:String>
    <x:String>Japanese Macaque</x:String>
  </Picker.Items>
</Picker>
```

The equivalent C# code is shown below:

```csharp
var picker = new Picker { Title = "Select a monkey", TitleColor = Color.Red };
picker.Items.Add("Baboon");
picker.Items.Add("Capuchin Monkey");
picker.Items.Add("Blue Monkey");
picker.Items.Add("Squirrel Monkey");
picker.Items.Add("Golden Lion Tamarin");
picker.Items.Add("Howler Monkey");
picker.Items.Add("Japanese Macaque");
```

In addition to adding data using the `Items.Add` method, data can also be inserted into the collection by using the `Items.Insert` method.

## Responding to item selection

A [`Picker`](xref:Xamarin.Forms.Picker) supports selection of one item at a time. When a user selects an item, the [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) event fires, and the [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) property is updated to an integer representing the index of the selected item in the list. The `SelectedIndex` property is a zero-based number indicating the item that the user selected. If no item is selected, which is the case when the `Picker` is first created and initialized, `SelectedIndex` will be -1.

> [!NOTE]
> Item selection behavior in a [`Picker`](xref:Xamarin.Forms.Picker) can be customized on iOS with a platform-specific. For more information, see [Controlling Picker Item Selection](~/xamarin-forms/platform/ios/picker-selection.md).

The following code example shows the `OnPickerSelectedIndexChanged` event handler method, which is executed when the [`SelectedIndexChanged`](xref:Xamarin.Forms.Picker.SelectedIndexChanged) event fires:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs e)
{
  var picker = (Picker)sender;
  int selectedIndex = picker.SelectedIndex;

  if (selectedIndex != -1)
  {
    monkeyNameLabel.Text = picker.Items[selectedIndex];
  }
}
```

This method obtains the [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) property value, and uses the value to retrieve the selected item from the [`Items`](xref:Xamarin.Forms.Picker.Items) collection. Because each item in the `Items` collection is a `string`, they can be displayed by a [`Label`](xref:Xamarin.Forms.Label) without requiring a cast.

> [!NOTE]
> A [`Picker`](xref:Xamarin.Forms.Picker) can be initialized to display a specific item by setting the [`SelectedIndex`](xref:Xamarin.Forms.Picker.SelectedIndex) property. However, the `SelectedIndex` property must be set after initializing the [`Items`](xref:Xamarin.Forms.Picker.Items) collection.

## Related links

- [Picker Demo (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-pickerdemo)
- [Picker](xref:Xamarin.Forms.Picker)
