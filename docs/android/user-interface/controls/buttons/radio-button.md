---
title: "RadioButton"
ms.prod: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
---

# RadioButton

In this section, you will create two mutually-exclusive radio buttons
(enabling one disables the other), using the
[`RadioGroup`](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/)
and
[`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)
widgets. When either radio button is pressed, a toast message will be
displayed.


Open the **Resources/layout/Main.axml** file and add two
[`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s, nested in
a
[`RadioGroup`](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) (inside the
[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<RadioGroup
  android:layout_width="fill_parent"
  android:layout_height="wrap_content"
  android:orientation="vertical">
  <RadioButton android:id="@+id/radio_red"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Red" />
  <RadioButton android:id="@+id/radio_blue"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:text="Blue" />
</RadioGroup>
```

It's important that the
[`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s are grouped
together by the
[`RadioGroup`](https://developer.xamarin.com/api/type/Android.Widget.RadioGroup/) element so
that no more than one can be selected at a time. This logic is
automatically handled by the Android system. When one
[`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)
within a group is selected, all others are automatically
deselected.

To do something when each
[`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/) is selected,
we need to write an event handler:

```csharp
private void RadioButtonClick (object sender, EventArgs e)
{
    RadioButton rb = (RadioButton)sender;
    Toast.MakeText (this, rb.Text, ToastLength.Short).Show ();
}
```

First, the sender that is passed in is cast into a RadioButton.
Then a
[`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
message displays the selected radio button's text.

Now, at the bottom of the
[`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle)
method, add the following:

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

This captures each of the
[`RadioButton`](https://developer.xamarin.com/api/type/Android.Widget.RadioButton/)s
from the layout and adds the newly-created event handlerto each.

Run the application.

**Tip:** If you need to change the state yourself (such as when loading a saved
[`CheckBoxPreference`](https://developer.xamarin.com/api/type/Android.Preferences.CheckBoxPreference/)),
use the
[`Checked`](https://developer.xamarin.com/api/property/Android.Widget.CompoundButton.Checked/)
property setter or
[`Toggle()`](https://developer.xamarin.com/api/member/Android.Widget.CompoundButton.Toggle/)
method.

*Portions of this page are modifications based on work created and
shared by the Android Open Source Project and used according to
terms described in the*
[*Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/). 
