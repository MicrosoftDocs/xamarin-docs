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

## Cells

A Cell is a specialized element used for items in a table and describes
  how each item in a list should be drawn. Cell derives from [`Element`](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/),
  from which VisualElement also derives. However Cell is not a visual element,
  it just describes a template for creating a visual element. [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/)
  provides base class and capabilities for all Xamarin.Forms cells. Cells are
  elements designed to be added to [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)
  or [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) controls.

To learn how to use and customize cells, refer to the [`ListView`](~/xamarin-forms/user-interface/listview/index.md)
 and [`TableView`](~/xamarin-forms/user-interface/tableview.md) documentation.

| Type | Description | Screenshot |
| ---- | ----------- | ---------- |
| [`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell) | A [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) with a label and a single line text entry field. | [![EntryCell Example](cells-images/EntryCell.png "EntryCell Example")](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryCellDemoPage.cs) |
| [`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell) | A [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) with a label and an on/off switch. | [![SwitchCell Example](cells-images/SwitchCell.png "SwitchCell Example")](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchCellDemoPage.cs) |
| [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell) | A [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/) with primary and secondary text. | [![TextCell Example](cells-images/TextCell.png "TextCell Example")](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TextCellDemoPage.cs) |
| [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell) | A [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) that also includes an image. | [![ImageCell Example](cells-images/ImageCell.png "ImageCell Example")](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageCellDemoPage.cs) |

## Related Links

- [Introduction To Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms FormsGallery sample](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms Samples](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API Documentation](https://developer.xamarin.com/api/root/Xamarin.Forms/)
