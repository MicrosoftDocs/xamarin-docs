---
title: "Xamarin.Android Performance"
description: "There are many techniques for increasing the performance of applications built with Xamarin.Android. Collectively these techniques can greatly reduce the amount of work being performed by a CPU, and the amount of memory consumed by an application. This article describes and discusses these techniques."
ms.prod: xamarin
ms.assetid: dc2e27f2-7f71-4d57-9cf9-165528276613
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
---

# Xamarin.Android Performance

_There are many techniques for increasing the performance of applications built with Xamarin.Android. Collectively these techniques can greatly reduce the amount of work being performed by a CPU, and the amount of memory consumed by an application. This article describes and discusses these techniques._

## Performance Overview

Poor application performance presents itself in many ways. It can make an application seem unresponsive, can cause slow scrolling, and can reduce battery life. However, optimizing performance involves more than just implementing efficient code. The user's experience of application performance must also be considered. For example, ensuring that operations execute without blocking the user from performing other activities can help to improve the user's experience.

There are a number of techniques for increasing the performance, and perceived performance, of applications built with Xamarin.Android. They include:

- [Optimize Layout Hierarchies](#optimizelayout)
- [Optimize List Views](#optimizelistviews)
- [Remove Event Handlers in Activities](#removeeventhandlers)
- [Limit the Lifespan of Services](#limitservices)
- [Release Resources when Notified](#releaseresources)
- [Release Resources when the User Interface is Hidden](#releaseresourcesuihidden)
- [Optimize Image Resources](#optimizeimages)
- [Dispose of Unused Image Resources](#disposeimages)
- [Avoid Floating-Point Arithmetic](#avoidfloats)
- [Dismiss Dialogs](#dismissdialogs)

> [!NOTE]
> Before reading this article you should first read [Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md), which discusses non-platform specific techniques to improve the memory usage and performance of applications built using the Xamarin platform.

<a name="optimizelayout"></a>

## Optimize Layout Hierarchies

Each layout added to an application requires initialization, layout, and drawing. The layout pass can be expensive when nesting [`LinearLayout`](xref:Android.Widget.LinearLayout) instances that use the `weight` parameter, because each child will be measured twice. Using nested instances of `LinearLayout` can lead to a deep view hierarchy, which can result in poor performance for layouts that are inflated multiple times, such as in a [`ListView`](xref:Android.Widget.ListView). Therefore, it's important that such layouts are optimized, as the performance benefits will then be multiplied.

For example, consider the [`LinearLayout`](xref:Android.Widget.LinearLayout) for a list view row that has an icon, a title, and a description. The `LinearLayout` will contain an [`ImageView`](xref:Android.Widget.ImageView) and a vertical `LinearLayout` that contains two [`TextView`](xref:Android.Widget.TextView) instances:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <LinearLayout
        android:orientation="vertical"
        android:layout_width="0dip"
        android:layout_weight="1"
        android:layout_height="fill_parent">
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:gravity="center_vertical"
            android:text="Mei tempor iuvaret ad." />
        <TextView
            android:layout_width="fill_parent"
            android:layout_height="0dip"
            android:layout_weight="1"
            android:singleLine="true"
            android:ellipsize="marquee"
            android:text="Lorem ipsum dolor sit amet." />
    </LinearLayout>
</LinearLayout>
```

This layout is 3-levels deep, and is wasteful when inflated for each [`ListView`](xref:Android.Widget.ListView) row. However, it can be improved by flattening the layout, as shown in the following code example:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="?android:attr/listPreferredItemHeight"
    android:padding="5dip">
    <ImageView
        android:id="@+id/icon"
        android:layout_width="wrap_content"
        android:layout_height="fill_parent"
        android:layout_alignParentTop="true"
        android:layout_alignParentBottom="true"
        android:layout_marginRight="5dip"
        android:src="@drawable/icon" />
    <TextView
        android:id="@+id/secondLine"
        android:layout_width="fill_parent"
        android:layout_height="25dip"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentBottom="true"
        android:layout_alignParentRight="true"
        android:singleLine="true"
        android:ellipsize="marquee"
        android:text="Lorem ipsum dolor sit amet." />
    <TextView
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_toRightOf="@id/icon"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:layout_above="@id/secondLine"
        android:layout_alignWithParentIfMissing="true"
        android:gravity="center_vertical"
        android:text="Mei tempor iuvaret ad." />
</RelativeLayout>
```

The previous 3-level hierarchy has been reduced to a 2-level hierarchy, and a single [`RelativeLayout`](xref:Android.Widget.RelativeLayout) has replaced two [`LinearLayout`](xref:Android.Widget.LinearLayout) instances. A significant performance increase will be gained when inflating the layout for each [`ListView`](xref:Android.Widget.ListView) row.

<a name="optimizelistviews"></a>

## Optimize List Views

Users expect smooth scrolling and fast load times for [`ListView`](xref:Android.Widget.ListView) instances. However, scrolling performance can suffer when each list view row contains deeply nested view hierarchies, or when list view rows contain complex layouts. However, there are techniques that can be used to avoid poor `ListView` performance:

- Reuse row views For more information, see [Reuse Row Views](#reuserowviews).
- Flatten layouts, where possible.
- Cache row content that is retrieved from a web service.
- Avoid image scaling.

Collectively these techniques can help to keep [`ListView`](xref:Android.Widget.ListView) instances scrolling smoothly.

<a name="reuserowviews"></a>

### Reuse Row Views

When displaying hundreds of rows in a [`ListView`](xref:Android.Widget.ListView), it would be a waste of memory to create hundreds of [`View`](xref:Android.Views.View) objects when only a small number of them are displayed on screen at once. Instead, only the `View` objects visible in the rows on screen can be loaded into memory, with the **content** being loaded into these reused objects. This prevents the instantiation of hundreds of additional objects, saving time and memory.

Therefore, when a row disappears from the screen its view can be placed in a queue for reuse, as shown in the following code example:

```csharp
public override View GetView(int position, View convertView, ViewGroup parent)
{
   View view = convertView; // re-use an existing view, if one is supplied
   if (view == null) // otherwise create a new one
       view = context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, null);
   // set view properties to reflect data for the given row
   view.FindViewById<TextView>(Android.Resource.Id.Text1).Text = items[position];
   // return the view, populated with data, for display
   return view;
}
```

As the user scrolls, the [`ListView`](xref:Android.Widget.ListView) calls the `GetView` override to request new views to display – if available it passes an unused view in the `convertView` parameter. If this value is `null` then the code creates a new [`View`](xref:Android.Views.View) instance, otherwise the `convertView` properties can be reset and reused.

For more information, see [Row View Re-Use](~/android/user-interface/layouts/list-view/populating.md#row-view-re-use) in [Populating a ListView with Data](~/android/user-interface/layouts/list-view/populating.md).

<a name="removeeventhandlers"></a>

## Remove Event Handlers in Activities

When an activity is destroyed in the Android runtime, it could still be alive in the Mono runtime. Therefore, remove event handlers to external objects in `Activity.OnPause` to prevent the runtime from keeping a reference to an activity that has been destroyed.

In an activity, declare event handler(s) at class level:

```csharp
EventHandler<UpdatingEventArgs> service1UpdateHandler;
```

Then implement the handlers in the activity, such as in `OnResume`:

```csharp
service1UpdateHandler = (object s, UpdatingEventArgs args) => {
    this.RunOnUiThread (() => {
        this.updateStatusText1.Text = args.Message;
    });
};
App.Current.Service1.Updated += service1UpdateHandler;
```

When the activity exits the running state, `OnPause` is called. In the `OnPause` implementation, remove the handlers as follows:

```csharp
App.Current.Service1.Updated -= service1UpdateHandler;
```

<a name="limitservices"></a>

## Limit the Lifespan of Services

When a service starts, Android keeps the service process running. This makes the process expensive because its memory can't be paged, or used elsewhere. Leaving a service running when it's not required therefore increases the risk of an application exhibiting poor performance due to memory constraints. It can also make application switching less efficient as it reduces the number of processes Android can cache.

The lifespan of a service can be limited by using an `IntentService`, which terminates itself once it's handled the intent that started it.

<a name="releaseresources"></a>

## Release Resources when Notified

During the application lifecycle, the [`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*) callback provides a notification when the device memory is low. This callback should be implemented to listen for the following memory level notifications:

- [`TrimMemoryRunningModerate`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningModerate) – the application *may* want to release some unneeded resources.
- [`TrimMemoryRunningLow`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningLow) – the application *should* release unneeded resources.
- [`TrimMemoryRunningCritical`](xref:Android.Content.ComponentCallbacks2.TrimMemoryRunningCritical) – the application *should* release as many non-critical processes as it can.

In addition, when the application process is cached, the following memory level notifications may be received by the [`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*) callback:

- [`TrimMemoryBackground`](xref:Android.Content.ComponentCallbacks2.TrimMemoryBackground) – release resources that can be quickly and efficiently rebuilt if the user returns to the app.
- [`TrimMemoryModerate`](xref:Android.Content.ComponentCallbacks2.TrimMemoryModerate) – releasing resources can help the system keep other processes cached for better overall performance.
- [`TrimMemoryComplete`](xref:Android.Content.ComponentCallbacks2.TrimMemoryComplete) – the application process will soon be terminated if more memory isn't soon recovered.

Notifications should be responded to by releasing resources based on the received level.

<a name="releaseresourcesuihidden"></a>

## Release Resources when the User Interface is Hidden

Release any resources used by the app's user interface when the user navigates to another app, as it can significantly increase Android's capacity for cached processes, which in turn can have an impact on the user experience quality.

To receive a notification when the user exits the UI, implement the [`OnTrimMemory`](xref:Android.App.Activity.OnTrimMemory*) callback in `Activity` classes and listen for the [`TrimMemoryUiHidden`](xref:Android.Content.ComponentCallbacks2.TrimMemoryUiHidden) level, which indicates that the UI is hidden from view. This notification will be received only when *all* the UI components of the application become hidden from the user. Releasing UI resources when this notification is received ensures that if the user navigates back from another activity in the app, the UI resources are still available to quickly resume the activity.

<a name="optimizeimages"></a>

## Optimize Image Resources

Images are some of the most expensive resources that applications use, and are often captured at high resolutions. Therefore, when displaying an image, display it at the resolution required for the device's screen. If the image is of a higher resolution than the screen, it should be scaled down.

For more information, see [Optimize Image Resources](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) in the [Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md) guide.

<a name="disposeimages"></a>

## Dispose of Unused Image Resources

To save on memory usage, it is a good idea to dispose of large image
resources that are no longer needed. However, it is important to ensure
that images are disposed of correctly. Instead of using an explicit
`.Dispose()` invocation, you can take advantage of
[using](/dotnet/csharp/language-reference/keywords/using-statement)
statements to ensure correct use of `IDisposable` objects. 

For example, the
[Bitmap](xref:Android.Graphics.Bitmap) class implements
`IDisposable`. Wrapping the instantiation of a `BitMap` object in a
`using` block ensures that it will be disposed of correctly
on exit from the block:

```csharp
using (Bitmap smallPic = BitmapFactory.DecodeByteArray(smallImageByte, 0, smallImageByte.Length))
{
    // Use the smallPic bit map here
}
```

For more information about releasing disposable resources, see
[Release IDisposable Resources](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).  

<a name="avoidfloats"></a>

## Avoid Floating-Point Arithmetic

On Android devices, floating-point arithmetic is about 2x slower than integer arithmetic. Therefore, replace floating-point arithmetic with integer arithmetic if possible. However, there's no execution time difference between `float` and `double` arithmetic on recent hardware.

> [!NOTE]
> Even for integer arithmetic, some CPUs lack hardware divide capabilities. Therefore, integer division and modulus operations are often performed in software.

<a name="dismissdialogs"></a>

## Dismiss Dialogs

When using the [`ProgressDialog`](xref:Android.App.ProgressDialog) class (or any dialog or alert), instead of calling the [`Hide`](xref:Android.App.Dialog.Hide*) method when the dialog's purpose is complete, call the [`Dismiss`](xref:Android.App.Dialog.Dismiss*) method. Otherwise, the dialog will still be alive and will leak the activity by holding a reference to it.

## Summary

This article described and discussed techniques for increasing the performance of applications built with Xamarin.Android. Collectively these techniques can greatly reduce the amount of work being performed by a CPU, and the amount of memory consumed by an application.

## Related Links

- [Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)