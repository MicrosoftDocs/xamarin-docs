---
title: "Xamarin.Forms Views"
description: "Xamarin.Forms views are the building blocks of cross-platform mobile user interfaces."
ms.topic: article
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/12/2016
---

# Xamarin.Forms Views

_Xamarin.Forms views are the building blocks of cross-platform mobile user interfaces._

<style>.tableimg { max-width: none !important;}</style>

## Views

Xamarin.Forms uses the word *View* to refer to visual objects such as buttons, labels or text entry boxes - which may be more commonly known as controls of widgets.

These UI elements are typically are subclasses of [`View`](https://developer.xamarin.com/api/type/Xamarin.Forms.View/).

<br clear="right" />

Xamarin.Forms supports:

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
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a>
    </td>
    <td valign="top">
    A visual control used to indicate that something is ongoing. This control gives a visual clue to the user that something is happening, without information about its progress.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ActivityIndicatorDemoPage.cs"><img src="views-images/ActivityIndicator.png" title="ActivityIndicator Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> used to draw a solid colored rectangle. BoxView is a useful stand-in for images or custom elements when doing initial prototyping. BoxView has a default size request of 40x40. If you need a different size, assign the <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.WidthRequest/">VisualElement.WidthRequest</a> and the <a href="https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.HeightRequest/">VisualElement.HeightRequest</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/BoxViewDemoPage.cs"><img src="views-images/BoxView.png" title="BoxView Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Button</a>
    </td>
    <td align="center" valign="top">
    A button <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> that reacts to touch events.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ButtonDemoPage.cs"><img src="views-images/Button.png" title="Button Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> that allows date picking. The visual representation of a DatePicker is very similar to the one of <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a>, except that a special control for picking a date appears in place of a keyboard
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/DatePickerDemoPage.cs"><img src="views-images/DatePicker.png" title="DatePicker Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Editor</a>
    </td>
    <td valign="top">
    A control that can edit multiple lines of text. For single line entries, see <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a>.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EditorDemoPage.cs"><img src="views-images/Editor.png" title="Editor Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a>
    </td>
    <td valign="top">
    A control that can edit a single line of text. Entry is a single line text entry. It is best used for collecting small discrete pieces of information, like usernames and passwords.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/EntryDemoPage.cs"><img src="views-images/Entry.png" title="Entry Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Image</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> that holds an image.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Image API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/images.md">Working with Images</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ImageDemoPage.cs">Demo source</a>
    </td>
    <td>
    <img src="views-images/Image.png" title="Image Example" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Label</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> that displays text in a read only format. A Label is used to display single-line text elements as well as multi-lines blocks of text.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/LabelDemoPage.cs"><img src="views-images/Label.png" title="Label Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a>
    </td>
    <td valign="top">
    An <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ItemsView%3CTVisual%3E/">ItemView</a> that displays a collection of data as a vertical list.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/listview/index.md">ListView documentation</a>
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ListViewDemoPage.cs"><img src="views-images/ListView.png" title="ListView Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/">OpenGLView</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> that displays OpenGL content.
    <ul>
      <li>Only works for iOS and Android projects (no Windows Phone support).
      <li>Requires a reference to the <b>OpenTK-1.0</b> assembly in the iOS and Android projects.</li>
      <li>Best suited to use in Shared Projects; if used in a PCL then a DependencyService will also be required.</li>
    </ul>
    </td>
    <td>
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.OpenGLView/"><img src="views-images/OpenGL.png" title="OpenGlView Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Picker</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> control for picking an element in a list. The visual representation of a Picker is similar to a <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a>, but a picker control appears in place of a keyboard.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/PickerDemoPage.cs"><img src="views-images/Picker.png" title="Picker Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> control indicating a progress.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/ProgressBarDemoPage.cs"><img src="views-images/ProgressBar.png" title="ProgressBar Example class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> control that provides a search box.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SearchBarDemoPage.cs"><img src="views-images/SearchBar.png" title="SearchBar Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Slider</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> control that inputs a linear value.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SliderDemoPage.cs"><img src="views-images/Slider.png" title="Slider Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Stepper</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> control that inputs a discrete value, constrained to a range.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/StepperDemoPage.cs"><img src="views-images/Stepper.png" title="Stepper Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">Switch</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> control that provides a toggled value.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/SwitchDemoPage.cs"><img src="views-images/Switch.png" title="Switch Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> that holds rows of <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Cell/">Cell</a>s.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/tableview.md">TableView documentation</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TableViewFormDemoPage.cs">Demo source</a>
    </td>
    <td>
    <img src="views-images/TableViewNewest.png" title="TableView Example" class="tableimg">
    </td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> control that provides time picking. The visual representation of a TimePicker is very similar to the one of <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a>, except that a special control for picking a time appears in place of a keyboard.
    </td>
    <td>
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/TimePickerDemoPage.cs"><img src="views-images/TimePicker.png" title="TimePicker Example" class="tableimg">
    </a></td>
  </tr>
  <tr>
    <td valign="top">
      <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView</a>
    </td>
    <td valign="top">
    A <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.View/">View</a> that presents HTML content.
    <br />
    <a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView API</a>
    <br />
    <a href="~/xamarin-forms/user-interface/webview.md">WebView documentation</a>
    <br />
    <a href="https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/WebViewDemoPage.cs">Demo source</a>
    </td>
    <td>
    <img src="views-images/WebView.png" title="WebView Example" class="tableimg">
    </td>
  </tr>
  </tbody>
</table>



## Related Links

- [Introduction To Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Xamarin.Forms Gallery (sample)](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Samples](https://developer.xamarin.com/samples/tag/Xamarin.Forms/)
- [Xamarin.Forms API Documentation](https://developer.xamarin.com/api/root/Xamarin.Forms/)
