---
title: "LinearLayout"
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/07/2018
---

# LinearLayout

[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
is a
[`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
that displays child
[`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
elements in a linear direction, either vertically or horizontally.

You should be careful about over-using the
[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).
If you begin nesting multiple
[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s,
you may want to consider using a
[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
instead.

Start a new project named **HelloLinearLayout**.

Open **Resources/Layout/Main.axml** and insert the following:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "match_parent"
      android:layout_height=    "match_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

Carefully inspect this XML. There is a root
[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
that defines its orientation to be vertical &ndash; all child
[`View`](https://developer.xamarin.com/api/type/Android.Views.View/)s
(of which it has two) will be stacked vertically. The first child
is another
[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
that uses a horizontal orientation and the second child is a
[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
that uses a vertical orientation. Each of these nested
[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s
contain several
[`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
elements, which are oriented with each other in the manner defined
by their parent
[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).

Now open **HelloLinearLayout.cs** and be sure it loads the
**Resources/Layout/Main.axml** layout in the
[`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
method:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

The [`SetContentView(int)`](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32))
method loads the layout file for the
[`Activity`](https://developer.xamarin.com/api/type/Android.App.Activity/), specified by the
resource ID &ndash; `Resources.Layout.Main` refers to the
**Resources/Layout/Main.axml** layout file.

Run the application. You should see the following:

[![Screenshot of app first LinearLayout arranged horizontally, second vertically](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

Notice how the XML attributes define each View's behavior. Try
experimenting with different values for `android:layout_weight` to see
how the screen real estate is distributed based on the weight of each
element. See the
[Common Layout Objects](http://developer.android.com/guide/topics/ui/declaring-layout.html)
document for more about how
[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
handles the `android:layout_weight` attribute.


## References

-   [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Portions of this page are modifications based on work created and shared by the
Android Open Source Project and used according to terms described in the*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).

