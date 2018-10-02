---
title: "Tabbed Layouts"
description: "An Overview of Tabbed Layouts in Android"
ms.prod: xamarin
ms.assetid: 1CFF590A-AC86-C3B3-36CA-A70248BC7F97
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/08/2017
---

# Tabbed Layouts


## Overview

Tabs are a popular user interface pattern in mobile applications 
because of their simplicity and usability. They provide a consistent, 
easy way to navigate between various screens in an application. Android 
has several API's for tabbed interfaces: 

-   **ActionBar** &ndash; This is part of a new set of API's that was 
    introduced in Android 3.0 (API level 11) with goal of providing a 
    consistent navigation and view-switching interface. It has been 
    back ported to Android 2.2 (API level 8) with the 
    [Android Support Library v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/). 

-   **PagerTabStrip** &ndash; Indicates the current, next, and previous pages 
    of a `ViewPager`. `ViewPager` is available only via 
    [Android Support Library v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).
     For more information about `PagerTabStrip`, see 
    [ViewPager](~/android/user-interface/controls/view-pager/index.md).

-   **Toolbar** &ndash; `Toolbar` is a newer and more flexible action 
    bar component that replaces `ActionBar`. `Toolbar` is available in
    Android 5.0 Lollipop or later, and it is also available for older versions of Android via the 
    [Android Support Library v7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) NuGet package. 
    `Toolbar` is currently the recommended action bar component to use in Android apps.
    For more information, see [Toolbar](~/android/user-interface/controls/tool-bar/index.md). 



## Related Links

- [Material Design - Tabs](https://material.io/guidelines/components/tabs.html)- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Android Support Library v7 AppCompat NuGet Package](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [v7 appcompat library](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
