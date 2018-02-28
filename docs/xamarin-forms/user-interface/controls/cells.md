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

<style>.tableimg { max-width: none !important;}</style>

## Cells

A Cell is a specialized element used for items in a table and describes
  how each item in a list should be drawn. Cell derives from [`Element`](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/),
  from which VisualElement also derives. However Cell is not a visual element,
  it just describes a template for creating a visual element. [`Cell`](https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/)
  provides base class and capabilities for all Xamarin.Forms cells. Cells are
  elements designed to be added to [`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)
  or [`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/) controls.

To learn how to use and customize cells, refer to the [ListView](~/xamarin-forms/user-interface/listview/index.md)
 and [TableView](~/xamarin-forms/user-interface/tableview.md) documentation.

<table align="center" border="1" cellpadding="1" cellspacing="1">
<thead>
    <th>
      <strong>Type</strong>
    </th>
    <th>
      <strong>Description</strong>
    </th>
    <th style="min-width:400px">
      <strong>Screenshot</strong>
    </th>
  </thead>
  <tbody>
    <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> with a label and a single line text entry field.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="cells-images/EntryCell.png" title="EntryCell Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> with a label and an on/off switch.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchCellDemoPage.cs"><img src="cells-images/SwitchCell.png" title="SwitchCell Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Xamarin.Forms.Cell</a> with primary and secondary text.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TextCellDemoPage.cs"><img src="cells-images/TextCell.png" title="TextCell Example" class="tableimg">
    </a></td>
  </tr>
      <tr>
    <td>
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">Text Cell</a> that also includes an image.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageCellDemoPage.cs"><img src="cells-images/ImageCell.png" title="ImageCell Example" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## Related Links

- [Introduction To Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms Gallery (sample)](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/)
- [Xamarin.Forms Samples](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms API Documentation](https://developer.xamarin.com/api/namespace/Xamarin.Forms/)
