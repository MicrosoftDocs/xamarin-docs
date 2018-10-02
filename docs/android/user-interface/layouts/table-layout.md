---
title: "TableLayout"
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
---

# TableLayout

[`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/)
is a
[`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
that displays child
[`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
elements in rows and columns.

Start a new project named **HelloTableLayout**.

Open the **Resources/Layout/Main.axml** file and insert the following:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:stretchColumns="1">

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Open..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-O"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save As..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-Shift-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Import..."
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Export..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-E"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Quit"
            android:padding="3dip"/>
    </TableRow>
</TableLayout>
```

Notice how this resembles the structure of an HTML table. The
[`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/)
element is like the HTML `<table>` element;
[`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/)
is like a `<tr>` element; but for the cells, you can use any kind
of [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
element. In this example, a 
[`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
is used for each cell. In between
some of the rows, there is also a basic
[`View`](https://developer.xamarin.com/api/type/Android.Views.View/),
which is used to draw a horizontal line.

Make sure your **HelloTableLayout** Activity loads this layout in the
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
resource ID &mdash; `Resource.Layout.Main` refers to the
**Resources/Layout/Main.axml** layout file.

Run the application. You should see the following:

[![Example screenshot of TableLayout app displaying multiple table rows](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)



## References

-   [`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 

-   [`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) 

-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Portions of this page are modifications based on work created and shared by the
Android Open Source Project and used according to terms described in the*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).
