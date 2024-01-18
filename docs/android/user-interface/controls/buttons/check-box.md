---
title: "CheckBox"
ms.service: xamarin
ms.assetid: A884AF10-D5EA-72CA-2301-B80CEC7FFBE7
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
description: Learn how to create a checkbox for selecting items using the CheckBox widget and how to add the CheckBox element.
---

# CheckBox

In this section, you will create a checkbox for selecting items, using the [`CheckBox`](xref:Android.Widget.CheckBox) widget. When the checkbox is pressed, a toast message will indicate the current state of the checkbox.

Open the **Resources/layout/Main.axml** file and add the [`CheckBox`](xref:Android.Widget.CheckBox) element (inside the [`LinearLayout`](xref:Android.Widget.LinearLayout)):

```xml
<CheckBox android:id="@+id/checkbox"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="check it out" />
```

To do something when the state is changed, add the following code to the end of the [`OnCreate()`](xref:Android.App.Activity.OnCreate*) method:

```csharp
CheckBox checkbox = FindViewById<CheckBox>(Resource.Id.checkbox);

checkbox.Click += (o, e) => {
    if (checkbox.Checked)
        Toast.MakeText (this, "Selected", ToastLength.Short).Show ();
    else
        Toast.MakeText (this, "Not selected", ToastLength.Short).Show ();
};
```

This captures the [`CheckBox`](xref:Android.Widget.CheckBox) element from the layout, then handles the Click event, which defines the action to be made when the checkbox is clicked. When clicked, the [`Checked`](xref:Android.Widget.CompoundButton.Checked) property is called to check the new state of the check box. If it has been checked, then a [`Toast`](xref:Android.Widget.Toast) displays the message "Selected",  otherwise it displays "Not selected". The [`CheckBox`](xref:Android.Widget.CheckBox) handles its own state changes, so you only need to query the current state.

Run it.

> [!TIP]
> If you need to change the state yourself (such as when loading a saved [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference), use the [`Checked`](xref:Android.Widget.CompoundButton.Checked) property setter or [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle) method.

*Portions of this page are modifications based on work created and shared by the Android Open Source Project and used according to terms described in the* [*Creative Commons 2.5 Attribution License*](https://creativecommons.org/licenses/by/2.5/).
