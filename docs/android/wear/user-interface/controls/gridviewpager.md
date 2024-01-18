---
title: "GridViewPager"
ms.service: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/02/2018
description: Learn how the GridViewPager sample demonstrates how to implement the 2D picker navigation pattern for Android Wear.
---

# GridViewPager

The [GridViewPager](/samples/xamarin/monodroid-samples/wear-gridviewpager) sample demonstrates
how to implement the 2D picker navigation pattern for Android Wear.

![Example screenshot of GridViewPager on a square display](gridviewpager-images/gridviewpager.png)

First add the
[Xamarin Android Wear Support](https://www.nuget.org/packages/Xamarin.Android.Wear/)
NuGet package to your project.

The layout XML looks like this:

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

Create a
[`GridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html)
(or subclass such as
[`FragmentGridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html)
to supply views to display as the user navigates.

The
[sample adapter](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs)
shows how to implement the required methods, including overrides for
`RowCount`, `GetColumnCount`, `GetBackground`, and `GetFragment`

Wire up the adapter as shown:

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```

## Related Links

- [Google's 2D Picker doc](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [android.support.wearable docs](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (sample)](/samples/xamarin/monodroid-samples/wear-gridviewpager)