---
title: "GridView"
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
---

# GridView

[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) is a
[`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
that displays items in a two-dimensional, scrollable grid. The grid
items are automatically inserted to the layout using a
[`ListAdapter`](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).

In this tutorial, you'll create a grid of image thumbnails. When an
item is selected, a toast message will display the position of the
image.

Start a new project named **HelloGridView**.

Find some photos you'd like to use, or
[download these sample
images](http://developer.android.com/shareables/sample_images.zip). Add
the image files to the project's **Resources/Drawable** directory. In
the **Properties** window, set the Build Action for each to
**AndroidResource**.

Open the **Resources/Layout/Main.axml** file and insert the following:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gridview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:columnWidth="90dp"
    android:numColumns="auto_fit"
    android:verticalSpacing="10dp"
    android:horizontalSpacing="10dp"
    android:stretchMode="columnWidth"
    android:gravity="center"
/>
```

This
[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) will fill the entire
screen. The attributes are rather self explanatory. For more
information about valid attributes, see the
[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) reference.

Open `HelloGridView.cs` and insert the following code for the
[`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
method:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    var gridview = FindViewById<GridView> (Resource.Id.gridview);
    gridview.Adapter = new ImageAdapter (this);

    gridview.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

After the **Main.axml** layout is set for the content view, the
[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) is captured from the
layout with
[`FindViewById`](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/). The
[`Adapter`](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/)
property is then used to set a custom adapter (`ImageAdapter`) as the
source for all items to be displayed in the grid. The `ImageAdapter` is
created in the next step.

To do something when an item in the grid is clicked, an anonymous
delegate is subscribed to the
[`ItemClick`](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) event.
It shows a
[`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/) that displays the index
position (zero-based) of the selected item (in a real world scenario,
the position could be used to get the full sized image for some other
task). Note that Java-style listener classes can be used instead of
.NET events.

Create a new class called `ImageAdapter` that subclasses
[`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
        context = c;
    }

    public override int Count {
        get { return thumbIds.Length; }
    }

    public override Java.Lang.Object GetItem (int position)
    {
        return null;
    }

    public override long GetItemId (int position)
    {
        return 0;
    }

    // create a new ImageView for each item referenced by the Adapter
    public override View GetView (int position, View convertView, ViewGroup parent)
    {
        ImageView imageView;

        if (convertView == null) {  // if it's not recycled, initialize some attributes
            imageView = new ImageView (context);
            imageView.LayoutParameters = new GridView.LayoutParams (85, 85);
            imageView.SetScaleType (ImageView.ScaleType.CenterCrop);
            imageView.SetPadding (8, 8, 8, 8);
        } else {
            imageView = (ImageView)convertView;
        }

        imageView.SetImageResource (thumbIds[position]);
        return imageView;
    }

    // references to our images
    int[] thumbIds = {
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7
    };
}
```

First, this implements some required methods inherited from
[`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/). The constructor
and the
[`Count`](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) property are
self-explanatory. Normally,
[`GetItem(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/)
should return the actual object at the specified position in the
adapter, but it's ignored for this example. Likewise,
[`GetItemId(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/)
should return the row id of the item, but it's not needed here.

The first method necessary is
[`GetView()`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/).
This method creates a new
[`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
for each image added to the `ImageAdapter`. When this is called, a
[`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
is passed in, which is normally a recycled object (at least after
this has been called once), so there's a check to see if the object
is null. If it *is* null, an
[`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
is instantiated and configured with desired properties for the
image presentation:

- [`LayoutParams`](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/)
  sets the height and width for the View&mdash;this ensures that,
  no matter the size of the drawable, each image is resized and
  cropped to fit in these dimensions, as appropriate.

- [`SetScaleType()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetScaleType/)
  declares that images should be cropped toward the center (if
  necessary).

- [`SetPadding(int, int, int, int)`](https://developer.xamarin.com/api/member/Android.Views.View.SetPadding/)
  defines the padding for all sides. (Note that, if the images have
  different aspect-ratios, then less padding will cause for more
  cropping of the image if it does not match the dimensions given
  to the ImageView.)

If the [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
passed to [`GetView()`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/)
is *not* null, then the local
[`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
is initialized with the recycled 
[`View`](https://developer.xamarin.com/api/type/Android.Views.View/) object.

At the end of the
[`GetView()`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/)
method, the `position` integer passed into the method is used to
select an image from the `thumbIds` array, which is set as the
image resource for the
[`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).

All that's left is to define the `thumbIds` array of drawable
resources.

Run the application. Your grid layout should look something like this:

[![Example screenshot of GridView displaying 15 images](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

Try experimenting with the behaviors of the
[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) and
[`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
elements by adjusting their properties. For example, instead of using
[`LayoutParams`](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) try using
[`SetAdjustViewBounds()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetAdjustViewBounds/).


## References

-   [`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).

*Portions of this page are modifications based on work created and shared by the
Android Open Source Project and used according to terms described in the*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).
