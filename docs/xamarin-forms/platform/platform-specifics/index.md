---
title: "Platform-Specifics"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects."
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
---

# Platform-Specifics

_Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects._

The following platform-specific functionality is built into Xamarin.Forms:

|iOS|Android|Windows|
|--- |--- |--- |
|[VisualElement.BlurEffect](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur)|[Application.WindowSoftInputModeAdjust](~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode)|[Page.ToolbarPlacement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement)|
|[NavigationPage.PrefersLargeTitles](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)|[ListView.IsFastScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll)|[MasterDetailPage.CollapsedPaneWidth and MasterDetailPage.CollapseStyle](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar)|
|[Page.UseSafeArea](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout)|[TabbedPage.IsSwipePagingEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging)|
|[NavigationPage.IsNavigationBarTranslucent](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar)|[Elevation.Elevation](~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation)|
|[NavigationPage.StatusBarTextColorMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode)|[Application.SendDisappearingEventOnPause, Application.SendAppearingEventOnResume, and Application.ShouldPreserveKeyboardOnResume](~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events)|
|[Entry.AdjustsFontSizeToFitWidth](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size)|
|[Picker.UpdateMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)|
|[Page.PrefersStatusBarHidden and Page.PreferredStatusBarUpdateAnimation](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility)|
|[ScrollView.ShouldDelayContentTouches](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches)|

The process for consuming a platform-specific through XAML, or through a fluent code API is as follows:

1. Add a `xmlns` declaration or `using` directive for the [`Xamarin.Forms.PlatformConfiguration`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) namespace.
1. Add a `xmlns` declaration or `using` directive for the namespace that contains the platform-specific functionality:
    1. On iOS, this is the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) namespace.
    1. On Android, this is the [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) namespace. For Android AppCompat, this is the [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) namespace.
    1. On the Universal Windows Platform, this is the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) namespace.
1. Apply the platform-specific from XAML, or from code with the `On<T>` fluent API. The value of `T` can be the [`iOS`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOS/), [`Android`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Android/), or [`Windows`](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.Windows/) types from the [`Xamarin.Forms.PlatformConfiguration`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/) namespace.

> [!NOTE]
> Note that attempting to consume a platform-specific on a platform where it is unavailable will not result in an error. Instead, the code will execute without the platform-specific being applied.

Platform-specifics consumed through the `On<T>` fluent code API return [`IPlatformElementConfiguration`](https://developer.xamarin.com/api/type/Xamarin.Forms.IPlatformElementConfiguration%3CTPlatform,TElement%3E/) objects. This allows multiple platform-specifics to be invoked on the same object with method cascading.

For more information about platform-specifics, see [Consuming Platform-Specifics](~/xamarin-forms/platform/platform-specifics/consuming/index.md) and [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/creating.md).


## Related Links

- [Consuming Platform-Specifics](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (sample)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration/)
