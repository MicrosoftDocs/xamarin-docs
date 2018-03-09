---
title: "Xamarin.Forms Cells"
description: "Xamarin.Forms cells can be added to ListViews and TableViews."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
---

# Xamarin.Forms Cells

_Xamarin.Forms cells can be added to ListViews and TableViews._

A *cell* is a specialized element used for items in a table and describes how each item in a list should be rendered. The [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) class derives from [`Element`](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/), from which [`VisualElement`](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/) also derives. A cell is not itself a visual element; it is instead a template for creating a visual element. 

`Cell` is used exclusively with [`ListView`](views.md#listView) and [`TableView`](views.md#tableView) controls. To learn how to use and customize cells, refer to the [`ListView`](~/xamarin-forms/user-interface/listview/index.md) and [`TableView`](~/xamarin-forms/user-interface/tableview.md) documentation.

## Cells

Xamarin.Forms supports the following cell types:

<a name="textCell" />

### TextCell

|     |     |
| --- | --- |
| A [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) displays one or two text strings. Set the [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Text/) property and, optionally, the [`Detail`](https://developer.xamarin.com/api/property/Xamarin.Forms.TextCell.Detail/) property to these text strings.<br /><br />[API Documentation](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) / [Guide](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#TextCell) | [![TextCell Example](cells-images/TextCell.png "TextCell Example")](cells-images/TextCell-Large.png#lightbox "TextCell Example")<br />[C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [XAML page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
|     |     |

### ImageCell

|     |     |
| --- | --- |
| The [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) displays the same information as [`TextCell`](#textCell) but includes a bitmap that you set with the [`Source`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) property.<br /><br />[API Documentation](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) / [Guide](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#ImageCell) | [![ImageCell Example](cells-images/ImageCell.png "ImageCell Example")](cells-images/ImageCell-Large.png#lightbox "ImageCell Example")<br />[C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [XAML page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
|     |     |

### SwitchCell

|     |     |
| --- | --- |
| The [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) contains text set with the [`Text`'](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCellText/) property and and on/off switch initially set with the Boolean [`On`](https://developer.xamarin.com/api/property/Xamarin.Forms.SwitchCell.On/) property. Handle the [`OnChanged`](https://developer.xamarin.com/api/event/Xamarin.Forms.SwitchCell.OnChanged/) event to be notified when the `On` property changes.<br /><br />[API Documentation](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) / [Guide](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell Example](cells-images/SwitchCell.png "SwitchCell Example")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell Example")<br />[C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [XAML page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
|     |     |

### EntryCell

|     |     |
| --- | --- |
| The [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) defines a [`Label`](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Label/) property that identifies the cell and a single line of editable text in the [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.EntryCell.Text/) property. Handle the [`Completed`](https://developer.xamarin.com/api/event/Xamarin.Forms.EntryCell.Completed/) event to be notified when the user has completed the text entry.<br /><br />[API Documentation](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) / [Guide](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell Example](cells-images/EntryCell.png "EntryCell Example")](cells-images/EntryCell-Large.png#lightbox "EntryCell Example")<br />[C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [XAML page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
|     |     |


## Related Links

- [Introduction To Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery sample](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms Samples](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API Documentation](https://developer.xamarin.com/api/root/Xamarin.Forms/)
