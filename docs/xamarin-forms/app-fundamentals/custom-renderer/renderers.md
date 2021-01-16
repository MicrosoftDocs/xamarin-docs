---
title: "Renderer Base Classes and Native Controls"
description: "Every Xamarin.Forms control has an accompanying renderer for each platform that creates an instance of a native control. This article lists the renderer and native control classes that implement each Xamarin.Forms page, layout, view, and cell."
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/09/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Renderer Base Classes and Native Controls

_Every Xamarin.Forms control has an accompanying renderer for each platform that creates an instance of a native control. This article lists the renderer and native control classes that implement each Xamarin.Forms page, layout, view, and cell._

With the exception of the `MapRenderer` class, the platform-specific renderers can be found in the following namespaces:

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **Android (AppCompat)** – Xamarin.Forms.Platform.Android.AppCompat
- **Android (FastRenderers)** - Xamarin.Forms.Platform.Android.FastRenderers
- **Universal Windows Platform (UWP)** – Xamarin.Forms.Platform.UWP

For more information about fast renderers, see [Xamarin.Forms Fast Renderers](~/xamarin-forms/internals/fast-renderers.md).

The `MapRenderer` class can be found in the following namespaces:

- **iOS** – Xamarin.Forms.Maps.iOS
- **Android** – Xamarin.Forms.Maps.Android
- **Universal Windows Platform (UWP)** – Xamarin.Forms.Maps.UWP

> [!NOTE]
> For information about creating custom renderers for Shell applications, see [Xamarin.Forms Shell Custom Renderers](~/xamarin-forms/app-fundamentals/shell/customrenderers.md).

## Pages

The following table lists the renderer and native control classes that implement each Xamarin.Forms [Page](~/xamarin-forms/user-interface/controls/pages.md) type:

|Page|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage)|PhoneFlyoutPageRenderer (iOS – Phone), TabletFlyoutPageRenderer (iOS – Tablet), MasterDetailRenderer (Android), FlyoutPageRenderer (Android AppCompat), FlyoutPageRenderer (UWP)|UIViewController (Phone), UISplitViewController (Tablet)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (Custom Control)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer (iOS and Android), NavigationPageRenderer (Android AppCompat), NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (Custom Control)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer (iOS and Android), TabbedPageRenderer (Android AppCompat), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (Pivot)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## Layouts

The following table lists the renderer and native control classes that implement each Xamarin.Forms [Layout](~/xamarin-forms/user-interface/controls/layouts.md) type:

|Layout|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|UIView|ViewGroup|CardView|Border|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|UIView|View|View|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|UIView|View|View|FrameworkElement|

## Views

The following table lists the renderer and native control classes that implement each Xamarin.Forms [View](~/xamarin-forms/user-interface/controls/views.md) type:

|Views|Renderer|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS and Android), BoxViewRenderer (UWP)|UIView|ViewGroup||Rectangle|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|UIButton|Button|AppCompatButton|Button|
|[`CarouselView`](xref:Xamarin.Forms.CarouselView)|CarouselViewRenderer|UICollectionView||RecyclerView|ListViewBase|
|[`CheckBox`](xref:Xamarin.Forms.CheckBox)|CheckBoxRenderer|UIButton||AppCompatCheckBox|CheckBox|
|[`CollectionView`](xref:Xamarin.Forms.CollectionView)|CollectionViewRenderer|UICollectionView||RecyclerView|ListViewBase|
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Ellipse`](xref:Xamarin.Forms.Shapes.Ellipse)|EllipseRenderer|CALayer|View||Ellipse|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||Image|
|[`ImageButton`](xref:Xamarin.Forms.ImageButton)|ImageButtonRenderer|UIButton||AppCompatImageButton|Button|
|[`IndicatorView`](xref:Xamarin.Forms.IndicatorView)|IndicatorViewRenderer|UIPageControl||LinearLayout||
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`Line`](xref:Xamarin.Forms.Shapes.Line)|LineRenderer|CALayer|View||Line|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)|MKMapView|MapView||MapControl|
|[`Path`](xref:Xamarin.Forms.Shapes.Path)|PathRenderer|CALayer|View||Path|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`Polygon`](xref:Xamarin.Forms.Shapes.Polygon)|PolygonRenderer|CALayer|View||Polygon|
|[`Polyline`](xref:Xamarin.Forms.Shapes.Polyline)|PolylineRenderer|CALayer|View||Polyline|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`RadioButton`](xref:Xamarin.Forms.RadioButton)|RadioButtonRenderer|UIButton||AppCompatRadioButton|RadioButton|
|[`Rectangle`](xref:Xamarin.Forms.Shapes.Rectangle)|RectangleRenderer|CALayer|View||Rectangle|
|[`RefreshView`](xref:Xamarin.Forms.RefreshView)|RefreshViewRenderer|UIView||SwipeRefreshLayout|RefreshContainer|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||Slider|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||Control|
|[`SwipeView`](xref:Xamarin.Forms.SwipeView)|SwipeViewRenderer|UIView||View|SwipeControl|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|Switch|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WkWebViewRenderer (iOS), WebViewRenderer (Android and UWP)|WkWebView|WebView||WebView|

## Cells

The following table lists the renderer and native control classes that implement each Xamarin.Forms [Cell](~/xamarin-forms/user-interface/controls/cells.md) type:

|Cells|Renderer|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITableViewCell with a UITextField|LinearLayout with a TextView and EditText|DataTemplate with a TextBox|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UITableViewCell with a UISwitch|Switch|DataTemplate with a Grid containing a TextBlock and ToggleSwitch|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|LinearLayout with two TextViews|DataTemplate with a StackPanel containing two TextBlocks|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UITableViewCell with a UIImage|LinearLayout with two TextViews and an ImageView|DataTemplate with a Grid containing an Image and two TextBlocks|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|View|DataTemplate with a ContentPresenter|

## Related links

- [Xamarin.Forms Fast Renderers](~/xamarin-forms/internals/fast-renderers.md)
- [Xamarin.Forms Shell Custom Renderers](~/xamarin-forms/app-fundamentals/shell/customrenderers.md)
