---
title: "Platform-Specifics"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects."
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
---

# Platform-Specifics

_Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects._

The following platform-specific functionality is provided for Xamarin.Forms views, pages, and layouts:

|iOS|Android|Windows|
|--- |--- |--- |
|[VisualElement.BlurEffect](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur)|[VisualElement.Elevation](~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation)|[VisualElement.AccessKey, VisualElement.AccessKeyPlacement, VisualElement.AccessKeyHorizontalOffset, and VisualElement.AccessKeyVerticalOffset](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#visualelement-accesskeys)|
|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#legacy-color-mode)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#legacy-color-mode)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#legacy-color-mode)|
|[VisualElement.IsShadowEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#drop-shadow)|

The following platform-specific functionality is provided for Xamarin.Forms views:

|iOS|Android|Windows|
|--- |--- |--- |
|[Entry.AdjustsFontSizeToFitWidth](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size)|[Button.UseDefaultPadding and Button.UseDefaultShadow](~/xamarin-forms/platform/platform-specifics/consuming/android.md#button-padding-shadow)|[InputView.DetectReadingOrderFromContent, Label.DetectReadingOrderFromContent](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#inputview-readingorder)|
|[Entry.CursorColor](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#entry-cursorcolor)|[Entry.ImeOptions](~/xamarin-forms/platform/platform-specifics/consuming/android.md#entry-imeoptions)|[ListView.SelectionMode](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#listview-selectionmode)|
|[ListView.SeparatorStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#listview-separatorstyle)|[ImageButton.IsShadowEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#imagebutton-drop-shadow)|[SearchBar.IsSpellCheckEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#searchbar-spellcheck)|
|[Picker.UpdateMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)|[ListView.IsFastScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll)|[WebView.IsJavaScriptAlertEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#webview-javascript-alert)|
|[Slider.UpdateOnTap](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#slider-updateontap)|[WebView.MixedContentMode](~/xamarin-forms/platform/platform-specifics/consuming/android.md#webview-mixed-content)|

The following platform-specific functionality is provided for Xamarin.Forms pages:

|iOS|Android|Windows|
|--- |--- |--- |
|[NavigationPage.HideSeparatorBar](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#navigationpage-hideseparatorbar)|[NavigationPage.BarHeight](~/xamarin-forms/platform/platform-specifics/consuming/android.md#navigationpage-barheight)|[MasterDetailPage.CollapsedPaneWidth and MasterDetailPage.CollapseStyle](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar)|
|[NavigationPage.IsNavigationBarTranslucent](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar)|[TabbedPage.IsSmoothScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-transition-animations)|[Page.ToolbarPlacement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement)|
|[NavigationPage.StatusBarTextColorMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode)|[TabbedPage.IsSwipePagingEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging)|[TabbedPage.HeaderIconsEnabled and TabbedPage.HeaderIconsSize](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#tabbedpage-icons)|
|[NavigationPage.PrefersLargeTitles](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)|[TabbedPage.ToolbarPlacement, TabbedPage.BarItemColor, and TabbedPage.BarSelectedItemColor](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar)|
|[Page.ModalPresentationStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#modal-page-presentation-style)|
|[Page.PrefersStatusBarHidden and Page.PreferredStatusBarUpdateAnimation](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility)|
|[Page.UseSafeArea](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout)|

The following platform-specific functionality is provided for Xamarin.Forms layouts:

|iOS|
|--- |
|[ScrollView.ShouldDelayContentTouches](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches)|

The following platform-specific functionality is provided for the Xamarin.Forms [`Application`](xref:Xamarin.Forms.Application) class:

|iOS|Android|
|--- |--- |
|[Application.HandleControlUpdatesOnMainThread](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#update-on-main-thread)|[Application.WindowSoftInputModeAdjust](~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode)|
|[Application.PanGestureRecognizerShouldRecognizeSimultaneously](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#simultaneous-pan-gesture)|[Application.SendDisappearingEventOnPause, Application.SendAppearingEventOnResume, and Application.ShouldPreserveKeyboardOnResume](~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events)|

## Consuming platform-specifics

The process for consuming a platform-specific through XAML, or through the fluent code API is as follows:

1. Add a `xmlns` declaration or `using` directive for the [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) namespace.
1. Add a `xmlns` declaration or `using` directive for the namespace that contains the platform-specific functionality:
    1. On iOS, this is the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace.
    1. On Android, this is the [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) namespace. For Android AppCompat, this is the [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) namespace.
    1. On the Universal Windows Platform, this is the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace.
1. Apply the platform-specific from XAML, or from code with the `On<T>` fluent API. The value of `T` can be the [`iOS`](xref:Xamarin.Forms.PlatformConfiguration.iOS), [`Android`](xref:Xamarin.Forms.PlatformConfiguration.Android), or [`Windows`](xref:Xamarin.Forms.PlatformConfiguration.Windows) types from the [`Xamarin.Forms.PlatformConfiguration`](xref:Xamarin.Forms.PlatformConfiguration) namespace.

> [!NOTE]
> Note that attempting to consume a platform-specific on a platform where it is unavailable will not result in an error. Instead, the code will execute without the platform-specific being applied.

Platform-specifics consumed through the `On<T>` fluent code API return [`IPlatformElementConfiguration`](xref:Xamarin.Forms.IPlatformElementConfiguration`2) objects. This allows multiple platform-specifics to be invoked on the same object with method cascading.

For more information about platform-specifics, see [Consuming Platform-Specifics](~/xamarin-forms/platform/platform-specifics/consuming/index.md) and [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/creating.md).

## Related Links

- [Consuming Platform-Specifics](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (sample)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](xref:Xamarin.Forms.PlatformConfiguration)
