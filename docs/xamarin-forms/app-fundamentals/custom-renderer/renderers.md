---
title: "Renderer Base Classes and Native Controls"
description: "Every Xamarin.Forms control has an accompanying renderer for each platform that creates an instance of a native control. This article lists the renderer and native control classes that implement each Xamarin.Forms page, layout, view, and cell."
ms.topic: article
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/15/2016
---

# Renderer Base Classes and Native Controls

_Every Xamarin.Forms control has an accompanying renderer for each platform that creates an instance of a native control. This article lists the renderer and native control classes that implement each Xamarin.Forms page, layout, view, and cell._

With the exception of the `MapRenderer` class, the platform-specific renderers can be found in the following namespaces:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **Windows Phone 8** – Xamarin.Forms.Platform.WinPhone
- **WinRT** – Xamarin.Forms.Platform.WinRT
- **Universal Windows Platform (UWP)** – Xamarin.Forms.Platform.UWP

The `MapRenderer` class can be found in the following namespaces:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Windows Phone 8** – Xamarin.Forms.Maps.WP8
- **WinRT** – Xamarin.Forms.Maps.WinRT
- **Universal Windows Platform (UWP)** – Xamarin.Forms.Maps.UWP

## Pages

The following table lists the renderer and native control classes that implement each Xamarin.Forms [Page](~/xamarin-forms/user-interface/controls/pages.md) type:

<table>
 <thead>
   <tr>
     <td><strong>Page</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Android (AppCompat)</strong></td>
     <td><strong>Windows Phone 8</strong</td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/">ContentPage</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md">PageRenderer</a></td>
     <td>UIViewController</td>
     <td>ViewGroup</td>
     <td></td>
     <td>Panel</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/">MasterDetailPage</a></td>
     <td><p>PhoneMasterDetailRenderer (iOS – Phone)</p><p>TabletMasterDetailPageRenderer (iOS – Tablet)</p><p>MasterDetailRenderer (Android)</p><p>MasterDetailPageRenderer (Android AppCompat)</p><p>MasterDetailRenderer (Windows Phone)</p><p>MasterDetailPageRenderer (WinRT)</p></td>
     <td><p>UIViewController (Phone)</p><p>UISplitViewController (Tablet)</p></td>
     <td>DrawerLayout (v4)</td>
     <td>DrawerLayout (v4)</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement (Custom Control)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/">NavigationPage</a></td>
     <td><p>NavigationRenderer (iOS and Android)</p><p>NavigationPageRenderer (Android AppCompat)</p><p>NavigationPageRenderer (Windows Phone 8 and WinRT)</p></td>
     <td>UIToolbar</td>
     <td>ViewGroup</td>
     <td>ViewGroup</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement (Custom Control)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/">TabbedPage</a></td>
     <td><p>TabbedRenderer (iOS and Android)</p><p>TabbedPageRenderer (Android AppCompat)</p><p>TabbedPageRenderer (Windows Phone 8 and WinRT)</p></td>
     <td>UIView</td>
     <td>ViewPager</td>
     <td>ViewPager</td>
     <td>UIElement (Pivot)</td>
     <td>FrameworkElement (Pivot)</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/">TemplatedPage</a></td>
     <td>PageRenderer</td>
     <td>UIViewController</td>
     <td>ViewGroup</td>
     <td></td>
     <td>Panel</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/">CarouselPage<a/></td>
     <td>CarouselPageRenderer</td>
     <td>UIScrollView</td>
     <td>ViewPager</td>
     <td>ViewPager</td>
     <td>UIElement (Panorama)</td>
     <td>FrameworkElement (FlipView)</td>
   </tr>
 </tbody>
</table>

## Layouts

The following table lists the renderer and native control classes that implement each Xamarin.Forms [Layout](~/xamarin-forms/user-interface/controls/layouts.md) type:

<table>
 <thead>
   <tr>
     <td><strong>Layout</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/">ContentPresenter</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>View</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/">ContentView</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>View</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/">Frame</a></td>
     <td>FrameRenderer</td>
     <td>UIView</td>
     <td>ViewGroup</td>
     <td>Border</td>
     <td>Border</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/">ScrollView</a></td>
     <td>ScrollViewRenderer</td>
     <td>UIScrollView</td>
     <td>ScrollView</td>
     <td>ScrollViewer</td>
     <td>ScrollViewer</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/">TemplatedView</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>View</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/">AbsoluteLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>View</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/">Grid</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>View</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/">RelativeLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>View</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/">StackLayout</a></td>
     <td>ViewRenderer</td>
     <td>UIView</td>
     <td>View</td>
     <td>FrameworkElement</td>
     <td>FrameworkElement</td>
   </tr>
 </tbody>
</table>

## Views

The following table lists the renderer and native control classes that implement each Xamarin.Forms [View](~/xamarin-forms/user-interface/controls/views.md) type:

<table>
 <thead>
   <tr>
     <td><strong>Views</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Android (AppCompat)</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/">ActivityIndicator</a></td>
     <td>ActivityIndicatorRenderer</td>
     <td>UIActivityIndicator</td>
     <td>ProgressBar</td>
     <td></td>
     <td>ProgressBar</td>
     <td>ProgressBar</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/">BoxView</a></td>
     <td><p>BoxRenderer (iOS and Android)</p><p>BoxViewRenderer (Windows Phone and WinRT)</p></td>
     <td>UIView</td>
     <td>ViewGroup</td>
     <td></td>
     <td>Rectangle</td>
     <td>Rectangle</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Button/">Button</a></td>
     <td>ButtonRenderer</td>
     <td>UIButton</td>
     <td>Button</td>
     <td>AppCompatButton</td>
     <td>Button</td>
     <td>Button</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/">CarouselView</a></td>
     <td>CarouselViewRenderer</td>
     <td>UIScrollView</td>
     <td>RecyclerView</td>
     <td></td>
     <td>FlipView</td>
     <td>FlipView</td>
   </tr>   
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/">DatePicker</a></td>
     <td>DatePickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>DatePicker</td>
     <td>DatePicker</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/">Editor</a></td>
     <td>EditorRenderer</td>
     <td>UITextView</td>
     <td>EditText</td>
     <td></td>
     <td>TextBox</td>
     <td>TextBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/">Entry</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/entry.md">EntryRenderer</a></td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>PhoneTextBox/PasswordBox</td>
     <td>TextBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Image/">Image</a></td>
     <td>ImageRenderer</td>
     <td>UIImageView</td>
     <td>ImageView</td>
     <td></td>
     <td>Image</td>
     <td>Image</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Label/">Label</a></td>
     <td>LabelRenderer</td>
     <td>UILabel</td>
     <td>TextView</td>
     <td></td>
     <td>TextBlock</td>
     <td>TextBlock</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/">ListView</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/listview.md">ListViewRenderer</a></td>
     <td>UITableView</td>
     <td>ListView</td>
     <td></td>
     <td>LongListSelector</td>
     <td>ListView</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/">Map</a></td>
     <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md">MapRenderer</a></td>
     <td>MKMapView</td>
     <td>MapView</td>
     <td></td>
     <td>Map</td>
     <td>MapControl</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/">Picker</a></td>
     <td>PickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td>EditText</td>
     <td>FrameworkElement</td>
     <td>ComboBox</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/">ProgressBar</a></td>
     <td>ProgressBarRenderer</td>
     <td>UIProgressView</td>
     <td>ProgressBar</td>
     <td></td>
     <td>ProgressBar</td>
     <td>ProgressBar</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/">SearchBar</a></td>
     <td>SearchBarRenderer</td>
     <td>UISearchBar</td>
     <td>SearchView</td>
     <td></td>
     <td>PhoneTextBox</td>
     <td><p>SearchBox (WinRT)</p><p>AutoSuggestBox (UWP)</p></td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/">Slider</a></td>
     <td>SliderRenderer</td>
     <td>UISlider</td>
     <td>SeekBar</td>
     <td></td>
     <td>Slider</td>
     <td>Slider</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/">Stepper</a></td>
     <td>StepperRenderer</td>
     <td>UIStepper</td>
     <td>LinearLayout</td>
     <td></td>
     <td>Border with a StackPanel and two Buttons</td>
     <td><p>UserControl (WinRT)</p><p>Control (UWP)</p></td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/">Switch</a></td>
     <td>SwitchRenderer</td>
     <td>UISwitch</td>
     <td>Switch</td>
     <td>SwitchCompat</td>
     <td>Border with a ToggleSwitchButton</td>
     <td>ToggleSwitch</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/">TableView</a></td>
     <td>TableViewRenderer</td>
     <td>UITableView</td>
     <td>ListView</td>
     <td></td>
     <td>Grid with a TextBlock and ListBox</td>
     <td>ListView</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/">TimePicker</a></td>
     <td>TimePickerRenderer</td>
     <td>UITextField</td>
     <td>EditText</td>
     <td></td>
     <td>TimePicker</td>
     <td>TimePicker</td>
   </tr>
   <tr>
     <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/">WebView</a></td>
     <td>WebViewRenderer</td>
     <td>UIWebView</td>
     <td>WebView</td>
     <td></td>
     <td>WebBrowser</td>
     <td>WebView</td>
   </tr>
 </tbody>
</table>

## Cells

The following table lists the renderer and native control classes that implement each Xamarin.Forms [Cell](~/xamarin-forms/user-interface/controls/cells.md) type:

<table>
 <thead>
   <tr>
     <td><strong>Cells</strong></td>
     <td><strong>Renderer</strong></td>
     <td><strong>iOS</strong></td>
     <td><strong>Android</strong></td>
     <td><strong>Windows Phone 8</strong></td>
     <td><strong>WinRT / UWP</strong></td>
   </tr>
 </thead>
 <tbody>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/">EntryCell</a></td>
   <td>EntryCellRenderer</td>
   <td>UITableViewCell with a UITextField</td>
   <td>LinearLayout with a TextView and EditText</td>
   <td>DataTemplate with a Grid containing a TextBlock and a PhoneTextBox</td>
   <td>DataTemplate with a TextBox</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/">SwitchCell</a></td>
   <td>SwitchCellRenderer</td>
   <td>UITableViewCell with a UISwitch</td>
   <td>Switch</td>
   <td>DataTemplate with a ToggleSwitch</td>
   <td>DataTemplate with a Grid containing a TextBlock and ToggleSwitch</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/">TextCell</a></td>
   <td>TextCellRenderer</td>
   <td>UITableViewCell</td>
   <td>LinearLayout with two TextViews</td>
   <td>DataTemplate with a Button containing a StackPanel with two TextBlocks</td>
   <td>DataTemplate with a StackPanel containing two TextBlocks</td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/">ImageCell</a></td>
   <td>ImageCellRenderer</td>
   <td>UITableViewCell with a UIImage</td>
   <td>LinearLayout with two TextViews and an ImageView</td>
   <td>DataTemplate with a Button containing a Grid with an Image, and a StackPanel containing two TextBlocks</td>
   <td>DataTemplate with a Grid containing an Image and two TextBlocks</td>  
   </td>
 </tr>
 <tr>
   <td><a href="https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/">ViewCell</a></td>
   <td><a href="~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md">ViewCellRenderer</a></td>
   <td>UITableViewCell</td>
   <td>View</td>
   <td>DataTemplate with a ContentPresenter</td>
   <td>DataTemplate with a ContentPresenter</td>  
   </td>
 </tr>
 </tbody>
</table>

## Summary

This article has listed the renderer and native control classes that implement each Xamarin.Forms page, layout, view, and cell. Every Xamarin.Forms control has an accompanying renderer for each platform that creates an instance of a native control.



## Related Links

- [Custom Renderers (Xamarin University Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
