---
title: "Using the RelativeLayout in Xamarin.Android"
description: "How to use RelativeLayout in a Xamarin.Android application"
ms.service: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/29/2018
---

# Xamarin.Android RelativeLayout

[`RelativeLayout`](xref:Android.Widget.RelativeLayout)
is a
[`ViewGroup`](xref:Android.Views.ViewGroup) that displays child
[`View`](xref:Android.Views.View)
elements in relative positions. The position of a
[`View`](xref:Android.Views.View) can
be specified as relative to sibling elements (such as to the left-of or
below a given element) or in positions relative to the
[`RelativeLayout`](xref:Android.Widget.RelativeLayout)
area (such as aligned to the bottom, left of center).

A [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
is a very powerful utility for designing a user interface because it
can eliminate nested
[`ViewGroup`](xref:Android.Views.ViewGroup)s. If you find
yourself using several nested
[`LinearLayout`](xref:Android.Widget.LinearLayout)
groups, you may be able to replace them with a single
[`RelativeLayout`](xref:Android.Widget.RelativeLayout).

Start a new project named **HelloRelativeLayout**.

Open the **Resources/Layout/Main.axml** file and insert the following:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background"
        android:layout_below="@id/label"/>
    <Button
        android:id="@+id/ok"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/entry"
        android:layout_alignParentRight="true"
        android:layout_marginLeft="10dip"
        android:text="OK" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toLeftOf="@id/ok"
        android:layout_alignTop="@id/ok"
        android:text="Cancel" />
</RelativeLayout>
```

Notice each of the `android:layout_*` attributes, such as
`layout_below`, `layout_alignParentRight`, and `layout_toLeftOf`.
When using a
[`RelativeLayout`](xref:Android.Widget.RelativeLayout), you can
use these attributes to describe how you want to position each
[`View`](xref:Android.Views.View). Each one of these attributes
define a different kind of relative position. Some attributes use the
resource ID of a sibling
[`View`](xref:Android.Views.View) to define its own relative
position. For example, the last
[`Button`](xref:Android.Widget.Button) is defined to lie to the
left-of and aligned-with-the-top-of the
[`View`](xref:Android.Views.View) identified by the ID `ok`
(which is the previous
[`Button`](xref:Android.Widget.Button)).

All of the available layout attributes are defined in
[`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams).

Make sure you load this layout in the
[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
method:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

The [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)
method loads the layout file for the
[`Activity`](xref:Android.App.Activity), specified by the
resource ID &mdash; `Resource.Layout.Main` refers to the
**Resources/Layout/Main.axml** layout file.

Run the application. You should see the following layout:

[![Screenshot of a relative layout with a TextView, EditText, and two buttons](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)

## Resources

- [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
- [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)
- [`TextView`](xref:Android.Widget.TextView)
- [`EditText`](xref:Android.Widget.EditText)
- [`Button`](xref:Android.Widget.Button)

_Portions of this page are modifications based on work created and shared by the
Android Open Source Project and used according to terms described in the
[Creative Commons 2.5 Attribution License](https://creativecommons.org/licenses/by/2.5/)._
