---
title: "Setting Accessibility Values on User Interface Elements"
description: "Xamarin.Forms allows accessibility values to be set on user interface elements by using attached properties from the AutomationProperties class, which in turn set native accessibility values. This article explains how to use the AutomationProperties class, so that a screen reader can speak about the elements on the page."
ms.prod: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
---

# Setting Accessibility Values on User Interface Elements

_Xamarin.Forms allows accessibility values to be set on user interface elements by using attached properties from the AutomationProperties class, which in turn set native accessibility values. This article explains how to use the AutomationProperties class, so that a screen reader can speak about the elements on the page._

## Overview

Xamarin.Forms allows accessibility values to be set on user interface elements via the following attached properties:

- `AutomationProperties.IsInAccessibleTree` – indicates whether the element is available to an accessible application. For more information, see [AutomationProperties.IsInAccessibleTree](#isinaccessibletree).
- `AutomationProperties.Name` – a short description of the element that serves as a speakable identifier for the element. For more information, see [AutomationProperties.Name](#name).
- `AutomationProperties.HelpText` – a longer description of the element, which can be thought of as tooltip text associated with the element. For more information, see [AutomationProperties.HelpText](#helptext).
- `AutomationProperties.LabeledBy` – allows another element to define accessibility information for the current element. For more information, see [AutomationProperties.LabeledBy](#labeledby).

These attached properties set native accessibility values so that a screen reader can speak about the element. For more information about attached properties, see [Attached Properties](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> Using the `AutomationProperties` attached properties may impact UI Test execution on Android. The [`AutomationId`](https://developer.xamarin.com/api/property/Xamarin.Forms.Element.AutomationId/), `AutomationProperties.Name` and `AutomationProperties.HelpText` properties will both set the native `ContentDescription` property, with the `AutomationProperties.Name` and `AutomationProperties.HelpText` property values taking precedence over the `AutomationId` value (if both `AutomationProperties.Name` and `AutomationProperties.HelpText` are set, the values will be concatenated). This means that any tests looking for `AutomationId` will fail if `AutomationProperties.Name` or `AutomationProperties.HelpText` are also set on the element. In this scenario, UI Tests should be altered to look for the value of `AutomationProperties.Name` or `AutomationProperties.HelpText`, or a concatenation of both.

Each platform has a different screen reader to narrate the accessibility values:

- iOS has VoiceOver. For more information, see [Test Accessibility on Your Device with VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) on developer.apple.com.
- Android has TalkBack. For more information, see [Testing Your App's Accessibility](https://developer.android.com/training/accessibility/testing.html#talkback) on developer.android.com.
- Windows has Narrator. For more information, see [Verify main app scenarios by using Narrator](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator/).

However, the exact behavior of a screen reader depends on the software and on the user's configuration of it. For example, most screen readers read the text associated with a control when it receives focus, enabling users to orient themselves as they move among the controls on the page. Some screen readers also read the entire application user interface when a page appears, which enables the user to receive all of the page's available informational content before attempting to navigate it.

Screen readers also read different accessibility values. In the sample application:

- VoiceOver will read the `Placeholder` value of the `Entry`, followed by instructions for using the control.
- TalkBack will read the `Placeholder` value of the `Entry`, followed by the `AutomationProperties.HelpText` value, followed by instructions for using the control.
- Narrator will read the `AutomationProperties.LabeledBy` value of the `Entry`, followed by instructions on using the control.

In addition, Narrator will prioritize `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, and then `AutomationProperties.HelpText`. On Android, TalkBack may combine the `AutomationProperties.Name` and `AutomationProperties.HelpText` values. Therefore, it's recommended that thorough accessibility testing is carried out on each platform to ensure an optimal experience.

<a name="isinaccessibletree" />

## AutomationProperties.IsInAccessibleTree

The `AutomationProperties.IsInAccessibleTree` attached property is a `boolean` that determines if the element is accessible, and hence visible, to screen readers. It must be set to `true` to use the other accessibility attached properties. This can be accomplished in XAML as follows:

```xaml
<Entry AutomationProperties.IsInAccessibleTree="true" />
```

Alternatively, it can be set in C# as follows:

```csharp
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
```

> [!NOTE]
> Note that the [`SetValue`](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) method can also be used to set the `AutomationProperties.IsInAccessibleTree` attached property – `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

<a name="name" />

## AutomationProperties.Name

The `AutomationProperties.Name` attached property value should be a short, descriptive text string that a screen reader uses to announce an element. This property should be set for elements that have a meaning that is important for understanding the content or interacting with the user interface. This can be accomplished in XAML as follows:

```xaml
<ActivityIndicator AutomationProperties.IsInAccessibleTree="true"
                   AutomationProperties.Name="Progress indicator" />
```

Alternatively, it can be set in C# as follows:

```csharp
var activityIndicator = new ActivityIndicator();
AutomationProperties.SetIsInAccessibleTree(activityIndicator, true);
AutomationProperties.SetName(activityIndicator, "Progress indicator");
```

> [!NOTE]
> Note that the [`SetValue`](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) method can also be used to set the `AutomationProperties.Name` attached property – `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

<a name="helptext" />

## AutomationProperties.HelpText

The `AutomationProperties.HelpText` attached property should be set to text that describes the user interface element, and can be thought of as tooltip text associated with the element. This can be accomplished in XAML as follows:

```xaml
<Button Text="Toggle ActivityIndicator"
        AutomationProperties.IsInAccessibleTree="true"
        AutomationProperties.HelpText="Tap to toggle the activity indicator" />
```

Alternatively, it can be set in C# as follows:

```csharp
var button = new Button { Text = "Toggle ActivityIndicator" };
AutomationProperties.SetIsInAccessibleTree(button, true);
AutomationProperties.SetHelpText(button, "Tap to toggle the activity indicator");
```

> [!NOTE]
> Note that the [`SetValue`](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) method can also be used to set the `AutomationProperties.HelpText` attached property – `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

On some platforms, for edit controls such as an [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/), the `HelpText` property can sometimes be omitted and replaced with placeholder text. For example, "Enter your name here" is a good candidate for the [`Entry.Placeholder`](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.Placeholder/) property that places the text in the control prior to the user's actual input.

<a name="labeledby" />

## AutomationProperties.LabeledBy

The `AutomationProperties.LabeledBy` attached property allows another element to define accessibility information for the current element. For example, a [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) next to an [`Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) can be used to describe what the `Entry` represents. This can be accomplished in XAML as follows:

```xaml
<Label x:Name="label" Text="Enter your name: " />
<Entry AutomationProperties.IsInAccessibleTree="true"
       AutomationProperties.LabeledBy="{x:Reference label}" />
```

Alternatively, it can be set in C# as follows:

```csharp
var nameLabel = new Label { Text = "Enter your name: " };
var entry = new Entry();
AutomationProperties.SetIsInAccessibleTree(entry, true);
AutomationProperties.SetLabeledBy(entry, nameLabel);
```

> [!NOTE]
> Note that the [`SetValue`](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/) method can also be used to set the `AutomationProperties.IsInAccessibleTree` attached property  – `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## Summary

This article explained how to set accessibility values on user interface elements in a Xamarin.Forms application by using attached properties from the `AutomationProperties` class. These attached properties set native accessibility values so that a screen reader can speak about the elements on the page.


## Related Links

- [Attached Properties](~/xamarin-forms/xaml/attached-properties.md)
- [Accessibility (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
