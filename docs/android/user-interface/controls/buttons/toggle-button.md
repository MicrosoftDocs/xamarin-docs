---
title: "ToggleButton"
description: Learn how to create a button used specifically for toggling between two states by using the ToggleButton widget.
ms.service: xamarin
ms.assetid: 9ADA8FCF-63ED-897A-DD56-D7D86535A92C
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
---

# ToggleButton

In this section, you'll create a button used specifically for toggling
between two states, using the
[`ToggleButton`](xref:Android.Widget.ToggleButton) widget. This
widget is an excellent alternative to radio buttons if you have two
simple states that are mutually exclusive ("on" and "off", for
example). Android 4.0 (API level 14) introduced an alternative to the
toggle button known as a
[`Switch`](xref:Android.Widget.Switch).

An example of a **ToggleButton** can be seen in the left hand pair of images,
while the right hand pair of images presents an example of a **Switch**:

![Examples of Switches and ToggleButtons in both on and off states](toggle-button-images/togglebutton-switch.png)  

Which control an application uses is a matter of style. Both widgets
are functionally equivalent.

Open the **Resources/layout/Main.axml** file and add the
[`ToggleButton`](xref:Android.Widget.ToggleButton) element
(inside the
[`LinearLayout`](xref:Android.Widget.LinearLayout)):

To do something when the state is changed, add the following code
to the end of the
[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
method:

```csharp
ToggleButton togglebutton = FindViewById<ToggleButton>(Resource.Id.togglebutton);

togglebutton.Click += (o, e) => {
    // Perform action on clicks
    if (togglebutton.Checked)
        Toast.MakeText(this, "Checked", ToastLength.Short).Show ();
    else
        Toast.MakeText(this, "Not checked", ToastLength.Short).Show ();
};
```

This captures the
[`ToggleButton`](xref:Android.Widget.ToggleButton) element
from the layout, and handles the Click event, which defines the
action to perform when the button is clicked. In this example, the
method checks the new state of the button, then shows a
[`Toast`](xref:Android.Widget.Toast) message that indicates
the current state.

Notice that the
[`ToggleButton`](xref:Android.Widget.ToggleButton) handles
its own state change between checked and unchecked, so you just ask
which it is.

Run the application.

> [!TIP]
> If you need to change the state yourself (such as
> when loading a saved
> [`CheckBoxPreference`](xref:Android.Preferences.CheckBoxPreference)),
> use the
> [`Checked`](xref:Android.Widget.CompoundButton.Checked)
> property setter or
> [`Toggle()`](xref:Android.Widget.CompoundButton.Toggle)
> method.

## Related Links

- [ToggleButton](https://developer.android.com/reference/android/widget/ToggleButton.html)
- [Switch](https://developer.android.com/reference/android/widget/Switch.html)
