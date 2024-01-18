---
title: "RadioButton"
description: Learn about radio buttons by creating two mutually-exclusive radio buttons using the RadioGroup and RadioButton widgets.
ms.service: xamarin
ms.assetid: 3C32EA3F-D917-C988-72C5-A17354DA791E
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
---

# RadioButton

In this section, you will create two mutually-exclusive radio buttons
(enabling one disables the other), using the
[`RadioGroup`](xref:Android.Widget.RadioGroup)
and
[`RadioButton`](xref:Android.Widget.RadioButton)
widgets. When either radio button is pressed, a toast message will be
displayed.

Open the **Resources/layout/Main.axml** file and add two
[`RadioButton`](xref:Android.Widget.RadioButton)s, nested in
a
[`RadioGroup`](xref:Android.Widget.RadioGroup) (inside the
[`LinearLayout`](xref:Android.Widget.LinearLayout)):

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
[`RadioButton`](xref:Android.Widget.RadioButton)s are grouped
together by the
[`RadioGroup`](xref:Android.Widget.RadioGroup) element so
that no more than one can be selected at a time. This logic is
automatically handled by the Android system. When one
[`RadioButton`](xref:Android.Widget.RadioButton)
within a group is selected, all others are automatically
deselected.

To do something when each
[`RadioButton`](xref:Android.Widget.RadioButton) is selected,
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
[`Toast`](xref:Android.Widget.Toast)
message displays the selected radio button's text.

Now, at the bottom of the
[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
method, add the following:

```csharp
RadioButton radio_red = FindViewById<RadioButton>(Resource.Id.radio_red);
RadioButton radio_blue = FindViewById<RadioButton>(Resource.Id.radio_blue);

radio_red.Click += RadioButtonClick;
radio_blue.Click += RadioButtonClick;
```

This captures each of the
[`RadioButton`](xref:Android.Widget.RadioButton)s
from the layout and adds the newly-created event handlerto each.

Run the application.

> [!TIP]
> If you need to change the state yourself (such as when loading a saved
> [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)),
> use the
> [`Checked`](xref:Android.Widget.CompoundButton.Checked)
> property setter or
> [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> method.

*Portions of this page are modifications based on work created and
shared by the Android Open Source Project and used according to
terms described in the*
[*Creative Commons 2.5 Attribution License*](https://creativecommons.org/licenses/by/2.5/). 
