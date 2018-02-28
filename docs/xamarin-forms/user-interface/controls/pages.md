---
title: "Xamarin.Forms Pages"
description: "Xamarin.Forms Pages represent cross-platform mobile app screens."
ms.topic: article
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
---

# Xamarin.Forms Pages

_Xamarin.Forms Pages represent cross-platform mobile app screens._

<style>.tableimg { max-width: none !important;}</style>

## Pages

The [`Page`](http://iosapi.xamarin.com/?link=T%3aXamarin.Forms.Page) class is a visual element that occupies most or all of the screen and contains a single child. A `Xamarin.Forms.Page` represents a View Controller in iOS or a Page in Windows Phone. On Android each page takes up the screen like an Activity, but Xamarin.Forms Pages are *not* Activities.

 [ ![](pages-images/pages-sml.png "Xamarin.Forms Page Types")](pages-images/pages.png "Xamarin.Forms Page Types")

<br clear="all" />

Xamarin.Forms supports:

<table align="center" border="1" cellpadding="1" cellspacing="1">
  <tr>
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
  </thead></tr>
  <tbody>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a>
    </td>
    <td align="center" valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a> displays a single <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a>, often a container such as a <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a> or a <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ContentPageDemoPage.cs"><img src="pages-images/ContentPage.png" title="ContentPage Example" class="tableimg">
    </a></td>
  </tr><tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">Page</a> that manages two panes of information.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/MasterDetailPageDemoPage.cs"><img src="pages-images/MasterDetailPage.png" title="MasterDetailPage Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">Page</a> that manages the navigation and user-experience of a stack of other pages.  
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/NavigationPageDemoPage.cs"><img src="pages-images/NavigationPage.png" title="NavigationPage Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">Page</a> that allows navigation between children pages, using tabs.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TabbedPageDemoPage.cs"><img src="pages-images/TabbedPage.png" title="TabbedPage Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">Page</a> that displays full-screen content with a control template, and the base class for <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a>.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/tree/master/Templates/ControlTemplates/"><img src="pages-images/TemplatedPage.png" title="TemplatedPage Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Page/">Page</a> allowing swipe gestures between subpages, like a gallery.
    </td>
    <td valign="top">
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CarouselPageDemoPage.cs"><img src="pages-images/CarouselPage.png" title="CarouselPage Example" class="tableimg">
    </a></td>
  </tr>
  </tbody>
</table>



## Related Links

- [Introduction To Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms Gallery (sample)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Samples](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms API Documentation](http://iosapi.xamarin.com/?link=N%3aXamarin.Forms)
