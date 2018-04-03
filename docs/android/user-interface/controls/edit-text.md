---
title: "Edit Text"
ms.prod: xamarin
ms.assetid: E513BCBC-438E-15E8-B83A-4B768A8E8B32
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
---

# Edit Text

In this section, you will create a text field for user input, using the
[EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) widget. Once text has
been entered into the field, the "Enter" key will display the text in a
toast message.

Open the <code>Resources\layout\main.xml</code> file and add the
[EditText](https://developer.xamarin.com/api/type/Android.Widget.EditText/) element (inside the
[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)):

```xml
<pre><code class=" syntax brush-C#">&lt;EditText
    android:id="@+id/edittext"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"/&gt;</code></pre>
```

To do something with the text that the user types, add the following
code to the end of the
[OnCreate](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/) method:

```csharp
EditText edittext = FindViewById<EditText>(Resource.Id.edittext);
edittext.KeyPress += (object sender, View.KeyEventArgs e) => {
 e.Handled = false;
 if (e.Event.Action == KeyEventActions.Down && e.KeyCode == Keycode.Enter) {
  Toast.MakeText (this, edittext.Text, ToastLength.Short).Show ();
  e.Handled = true;
         }
};
```

This captures the
[`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/) element from the
layout and sets a
[`KeyPress`](https://developer.xamarin.com/api/event/Android.Views.View.KeyPress/) handler, which
defines the action to be made when a key is pressed while the widget
has focus. In this case, the method is defined to listen for the Enter
key (when pressed down), then pop up a
[`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/) message with the text that
has been entered. The
[`Handled`](https://developer.xamarin.com/api/property/Android.Views.View+KeyEventArgs.Handled/)
property should always be `true` if the event has been handled, so that
the event doesn't bubble-up (which would result in a carriage return in
the text field).

Run the application.

*Portions of this page are modifications based on work created and* [ *shared by the Android Open Source Project*](http://code.google.com/policies.html) *and used according to terms described in the* [ *Creative Commons 2.5 Attribution License*](http://creativecommons.org/licenses/by/2.5/) *. This tutorial is based on the* [ *Android Form Stuff tutorial*](http://developer.android.com/resources/tutorials/views/hello-formstuff.html) *.*



## Related Links

- [EditTextSample](https://developer.xamarin.com/samples/monodroid/UserInterface/EditTextSample/)
