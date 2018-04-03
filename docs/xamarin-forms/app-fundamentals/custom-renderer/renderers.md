---
title: "Renderer Base Classes and Native Controls"
description: "Every Xamarin.Forms control has an accompanying renderer for each platform that creates an instance of a native control. This article lists the renderer and native control classes that implement each Xamarin.Forms page, layout, view, and cell."
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
- **Universal Windows Platform (UWP)** – Xamarin.Forms.Platform.UWP

The `MapRenderer` class can be found in the following namespaces:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Universal Windows Platform (UWP)** – Xamarin.Forms.Maps.UWP

## Pages

The following table lists the renderer and native control classes that implement each Xamarin.Forms [Page](~/xamarin-forms/user-interface/controls/pages.md) type:

|Page|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/)|PhoneMasterDetailRenderer (iOS – Phone), TabletMasterDetailPageRenderer (iOS – Tablet), MasterDetailRenderer (Android), MasterDetailPageRenderer (Android AppCompat), MasterDetailPageRenderer (UWP)|UIViewController (Phone), UISplitViewController (Tablet)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (Custom Control)|
|[`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)|NavigationRenderer (iOS and Android), NavigationPageRenderer (Android AppCompat), NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (Custom Control)|
|[`TabbedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/)|TabbedRenderer (iOS and Android), TabbedPageRenderer (Android AppCompat), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (Pivot)|
|[`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselPage/)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## Layouts

The following table lists the renderer and native control classes that implement each Xamarin.Forms [Layout](~/xamarin-forms/user-interface/controls/layouts.md) type:

|Layout|Renderer|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)|ViewRenderer|UIView|View|FrameworkElement|
|[`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)|ViewRenderer|UIView|View|FrameworkElement|
|[`Frame`](https://developer.xamarin.com/api/type/Xamarin.Forms.Frame/)|FrameRenderer|UIView|ViewGroup|Border|
|[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)|ViewRenderer|UIView|View|FrameworkElement|
|[`AbsoluteLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.AbsoluteLayout/)|ViewRenderer|UIView|View|FrameworkElement|
|[`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)|ViewRenderer|UIView|View|FrameworkElement|
|[`RelativeLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.RelativeLayout/)|ViewRenderer|UIView|View|FrameworkElement|
|[`StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)|ViewRenderer|UIView|View|FrameworkElement|

## Views

The following table lists the renderer and native control classes that implement each Xamarin.Forms [View](~/xamarin-forms/user-interface/controls/views.md) type:

|Views|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/)|BoxRenderer (iOS and Android), BoxViewRenderer (Windows Phone and WinRT)|UIView|ViewGroup||Rectangle|
|[`Button`](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/)|ButtonRenderer|UIButton|Button|AppCompatButton|Button|
|[`CarouselView`](https://developer.xamarin.com/api/type/Xamarin.Forms.CarouselView/)|CarouselViewRenderer|UIScrollView|RecyclerView||FlipView|
|[`DatePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.DatePicker/)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](https://developer.xamarin.com/api/type/Xamarin.Forms.Editor/)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)|ImageRenderer|UIImageView|ImageView||Image|
|[`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.ProgressBar/)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/)|SliderRenderer|UISlider|SeekBar||Slider|
|[`Stepper`](https://developer.xamarin.com/api/type/Xamarin.Forms.Stepper/)|StepperRenderer|UIStepper|LinearLayout||Control|
|[`Switch`](https://developer.xamarin.com/api/type/Xamarin.Forms.Switch/)|SwitchRenderer|UISwitch|Switch|SwitchCompat|ToggleSwitch|
|[`TableView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TableView/)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](https://developer.xamarin.com/api/type/Xamarin.Forms.TimePicker/)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/)|WebViewRenderer|UIWebView|WebView||WebView|

## Cells

The following table lists the renderer and native control classes that implement each Xamarin.Forms [Cell](~/xamarin-forms/user-interface/controls/cells.md) type:

|Cells|Renderer|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.EntryCell/)|EntryCellRenderer|UITableViewCell with a UITextField|LinearLayout with a TextView and EditText|DataTemplate with a TextBox|
|[`SwitchCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.SwitchCell/)|SwitchCellRenderer|UITableViewCell with a UISwitch|Switch|DataTemplate with a Grid containing a TextBlock and ToggleSwitch|
|[`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/)|TextCellRenderer|UITableViewCell|LinearLayout with two TextViews|DataTemplate with a StackPanel containing two TextBlocks|
|[`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/)|ImageCellRenderer|UITableViewCell with a UIImage|LinearLayout with two TextViews and an ImageView|DataTemplate with a Grid containing an Image and two TextBlocks|
|[`ViewCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|View|DataTemplate with a ContentPresenter|

## Summary

This article has listed the renderer and native control classes that implement each Xamarin.Forms page, layout, view, and cell. Every Xamarin.Forms control has an accompanying renderer for each platform that creates an instance of a native control.

## Related Links

- [Custom Renderers (Xamarin University Video)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
