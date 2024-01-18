---
title: "Xamarin.Forms Cells"
description: "Xamarin.Forms cells can be added to ListViews and TableViews. This article lists the cells included in Xamarin.Forms."
ms.service: xamarin
ms.assetid: 77DA0C89-35D6-4C09-A072-3ADE53FD56CF
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Cells

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/formsgallery)

_Xamarin.Forms cells can be added to ListViews and TableViews._

A *cell* is a specialized element used for items in a table and describes how each item in a list should be rendered. The [`Cell`](xref:Xamarin.Forms.Cell) class derives from [`Element`](xref:Xamarin.Forms.Element), from which [`VisualElement`](xref:Xamarin.Forms.Element) also derives. A cell is not itself a visual element; it is instead a template for creating a visual element.

`Cell` is used exclusively with [`ListView`](xref:Xamarin.Forms.ListView) and [`TableView`](xref:Xamarin.Forms.TableView) controls. To learn how to use and customize cells, refer to the [`ListView`](~/xamarin-forms/user-interface/listview/index.md) and [`TableView`](~/xamarin-forms/user-interface/tableview.md) documentation.

## Cells

Xamarin.Forms supports the following cell types:

| Type | Description | Appearance |
| --- | --- | --- |
| `TextCell` | A [`TextCell`](xref:Xamarin.Forms.TextCell) displays one or two text strings. Set the [`Text`](xref:Xamarin.Forms.TextCell.Text) property and, optionally, the [`Detail`](xref:Xamarin.Forms.TextCell.Detail) property to these text strings.<br /><br />[API Documentation](xref:Xamarin.Forms.TextCell) / [Guide](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#textcell) | [![TextCell Example](cells-images/TextCell.png "TextCell Example")](cells-images/TextCell-Large.png#lightbox "TextCell Example")<br />[C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) / [XAML page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TextCellDemoPage.xaml) |
| `ImageCell` | The [`ImageCell`](xref:Xamarin.Forms.ImageCell) displays the same information as [`TextCell`](xref:Xamarin.Forms.TextCell) but includes a bitmap that you set with the [`Source`](xref:Xamarin.Forms.Image.Source) property.<br /><br />[API Documentation](xref:Xamarin.Forms.ImageCell) / [Guide](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#imagecell) | [![ImageCell Example](cells-images/ImageCell.png "ImageCell Example")](cells-images/ImageCell-Large.png#lightbox "ImageCell Example")<br />[C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) / [XAML page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageCellDemoPage.xaml) |
| `SwitchCell` | The [`SwitchCell`](xref:Xamarin.Forms.SwitchCell) contains text set with the [`Text`](xref:Xamarin.Forms.SwitchCell.Text) property and an on/off switch initially set with the Boolean [`On`](xref:Xamarin.Forms.SwitchCell.On) property. Handle the [`OnChanged`](xref:Xamarin.Forms.SwitchCell.OnChanged) event to be notified when the `On` property changes.<br /><br />[API Documentation](xref:Xamarin.Forms.SwitchCell) / [Guide](~/xamarin-forms/user-interface/tableview.md#switchcell) | [![SwitchCell Example](cells-images/SwitchCell.png "SwitchCell Example")](cells-images/SwitchCell-Large.png#lightbox "SwitchCell Example")<br />[C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) / [XAML page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchCellDemoPage.xaml) |
| `EntryCell` | The [`EntryCell`](xref:Xamarin.Forms.EntryCell) defines a [`Label`](xref:Xamarin.Forms.EntryCell.Label) property that identifies the cell and a single line of editable text in the [`Text`](xref:Xamarin.Forms.EntryCell.Text) property. Handle the [`Completed`](xref:Xamarin.Forms.EntryCell.Completed) event to be notified when the user has completed the text entry.<br /><br />[API Documentation](xref:Xamarin.Forms.EntryCell) / [Guide](~/xamarin-forms/user-interface/tableview.md#entrycell) | [![EntryCell Example](cells-images/EntryCell.png "EntryCell Example")](cells-images/EntryCell-Large.png#lightbox "EntryCell Example")<br />[C# code for this page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) / [XAML page](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryCellDemoPage.xaml) |
| | | |

## Related links

- [Xamarin.Forms FormsGallery sample](/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms Samples](/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API Documentation](/dotnet/api/xamarin.forms?view=xamarin-forms&preserve-view=true)