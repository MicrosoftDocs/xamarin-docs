---
title: "CheckBox"
ms.prod: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
---

# CheckBox

In this section, you will create a checkbox for selecting items, using the
[`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox)
widget. When the checkbox is pressed, a toast message will indicate the
current state of the checkbox.

Open the **Resources/layout/Main.axml** file and add the
[`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/) element (inside
the [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

To do something when the state is changed, add the following code
to the end of the
[`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
method:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

This captures the
[`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)
element from the layout, then handles the Click event, which
defines the action to be made when the checkbox is clicked. When
clicked, the
[`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)
property is called to check the new state of the check box. If it
has been checked, then a
[`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
displays the message "Selected", otherwise it displays
"Not selected". The
[`CheckBox`](https://developer.xamarin.com/api/type/Android.Widget.CheckBox/)
handles its own state changes, so you only need to query the
current state.

Run it.

**Tip:** If you need to change the state yourself (such as when loading
a saved
[`CheckBoxPreference`](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference),
use the
[`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked)
property setter or
[`Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle)
method.

*Portions of this page are modifications based on work created and
shared by the Android Open Source Project and used according to terms
described in the*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/).
