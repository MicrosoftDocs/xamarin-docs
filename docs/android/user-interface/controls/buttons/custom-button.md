---
title: "Custom Button"
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
---

# Custom Button

In this section, you will create a button with a custom image instead
of text, using the [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/) widget 
and an XML file that defines three different images to use for the
different button states. When the button is pressed, a short message
will be displayed.

Right-click and download the three images below, then copy them to
the **Resources/drawable** directory of your project. These will be
used for the different button states.

 [![Green Android icon for normal state](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox)
 [![Orange Android icon for focused state](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox)
 [![Yellow Android icon for pressed state](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

Create a new file in the **Resources/drawable** directory named
**android_button.xml**. Insert the following XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/android_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/android_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/android_normal" />
</selector>
```

This defines a single drawable resource, which will change its
image based on the current state of the button. The first `<item>`
defines **android_pressed.png** as the image when the button is
pressed (it's been activated); the second `<item>` defines
**android_focused.png** as the image when the button is focused (when
the button is highlighted using the trackball or directional pad);
and the third `<item>` defines **android_normal.png** as the image
for the normal state (when neither pressed nor focused). This XML
file now represents a single drawable resource and when referenced
by a [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)
for its background, the image displayed will change based on these
three states.


> [!NOTE]
> The order of the `<item>` elements is important. When
> this drawable is referenced, the `<item>`s are traversed in-order
> to determine which one is appropriate for the current button state.
> Because the "normal" image is last, it is only applied
> when the conditions `android:state_pressed` and
> `android:state_focused` have both evaluated false.

Open the **Resources/layout/Main.axml** file and add the
[`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/) element:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

The `android:background` attribute specifies the drawable resource
to use for the button background (which, when saved at
**Resources/drawable/android.xml**, is referenced as
`@drawable/android`). This replaces the normal background image
used for buttons throughout the system. In order for the drawable
to change its image based on the button state, the image must be
applied to the background.

To make the button do something when pressed, add the following
code at the end of the
[`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/)
method:

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

This captures the [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)
from the layout, then adds a [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
message to be displayed when the [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)
is clicked.

Now run the application.


*Portions of this page are modifications based on work created and
shared by the Android Open Source Project and used according to
terms described in the*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).
