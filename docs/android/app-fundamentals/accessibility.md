---
title: "Accessibility on Android"
ms.prod: xamarin
ms.assetid: 157F0899-4E3E-4538-90AF-B59B8A871204
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/28/2018
---

# Accessibility on Android

This page describes how to use the Android Accessibility APIs
to build apps according to the
[accessibility checklist](~/cross-platform/app-fundamentals/accessibility.md).
Refer to the
[iOS accessibility](~/ios/app-fundamentals/accessibility.md)
and [OS X accessibility](~/mac/app-fundamentals/accessibility.md) pages for
other platform APIs.


## Describing UI Elements

Android provides a `ContentDescription` property that is used by
screen reading APIs to provide an accessible description of the control's purpose.

The content description can be set in either C# or in the AXML layout file.

**C#**

The description can be set in code to any string (or a string resource):

```csharp
saveButton.ContentDescription = "Save data";
```

**AXML layout**

In XML layouts use the `android:contentDescription` attribute:

```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```

### Use Hint for TextView

For `EditText` and `TextView` controls for data input, use the `Hint` property
to provide a description of what input is expected (instead of `ContentDescription`).
When some text has been entered, the text itself will be "read" instead of the hint.

**C#**

Set the `Hint` property in code:

```csharp
someText.Hint = "Enter some text"; // displays (and is "read") when control is empty
```

**AXML layout**

In XML layout files use the `android:hint` attribute:

```xml
<EditText
    android:id="@+id/someText"
    android:hint="Enter some text" />
```


### LabelFor links input fields with labels

To associate a label with a data input control, use the `LabelFor`
property to

**C#**

In C#, set the `LabelFor` property to the resource ID of the
control that this content describes (typically this property is
set on a label and references some other input control):

```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```

**AXML layout**

In layout XML use the `android:labelFor` property to reference
another control's identifier:

```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

### Announce for Accessibility

Use the `AnnounceForAccessibility` method on any view control to
communicate an event or status change to users when accessibility is
enabled. This method isn't required for most operations where the built-in
narration provides sufficient feedback, but should be used where additional
information would be helpful for the user.

The code below shows a simple example calling `AnnounceForAccessibility`:

```csharp
button.Click += delegate {
  button.Text = string.Format ("{0} clicks!", count++);
  button.AnnounceForAccessibility (button.Text);
};
```

## Changing Focus Settings

Accessible navigation relies on controls having focus to aid the
user in understanding what operations are available. Android provides
a `Focusable` property which can tag controls as specifically able
to receive focus during navigation.

**C#**

To prevent a control from gaining focus with C#, set the `Focusable`
property to `false`:

```csharp
label.Focusable = false;
```

**AXML layout**

In layout XML files set the `android:focusable` attribute:

```xml
<android:focusable="false" />
```

You can also control focus order with the
`nextFocusDown`, `nextFocusLeft`, `nextFocusRight`, `nextFocusUp`
attributes, typically set in the layout AXML. Use these attributes
to ensure the user can navigate easily through the controls
on the screen.


## Accessibility and Localization

In the examples above the hint and content description are set
directly to the display value. It is preferable to use values
in a **Strings.xml** file, such as this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
	<string name="enter_info">Enter some text</string>
	<string name="save_info">Save data</string>
</resources>
```

Using text from a strings file is shown below in C# and AXML layout files:

**C#**

Instead of using string literals in code, look up translated values from
strings files with `Resources.GetText`:

```csharp
someText.Hint = Resources.GetText (Resource.String.enter_info);
saveButton.ContentDescription = Resources.GetText (Resource.String.save_info);
```

**AXML**

In layout XML accessibility attributes like `hint` and `contentDescription`
can be set to a string identifier:

```xml
<TextView
    android:id="@+id/someText"
    android:hint="@string/enter_info" />
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="@string/save_info" />
```

The benefit of storing text in a separate file is multiple language
translations of the file can be provided in your app. See the
[Android localization guide](~/android/app-fundamentals/localization.md)
to learn how add localized string files to an application project.


## Testing Accessibility

Follow [these steps](http://developer.android.com/training/accessibility/testing.html#how-to)
to enable TalkBack and Explore by Touch to test accessibility on Android devices.

You may need to install [TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback)
from Google Play if it does not appear in **Settings > Accessibility**.


## Related Links

- [Cross-platform Accessibility](~/cross-platform/app-fundamentals/accessibility.md)
- [Android Accessibility APIs](http://developer.android.com/guide/topics/ui/accessibility/index.html)
