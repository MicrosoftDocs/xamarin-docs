---
title: "Automation Properties"
description: "This article explains how to use the AutomationProperties class in a Xamarin.Forms application, so that a screen reader can speak about the elements on the page."
ms.service: xamarin
ms.assetid: c0bb6893-fd26-47e7-88e5-3c333c9f786c
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Automation Properties in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)

_Xamarin.Forms allows accessibility values to be set on user interface elements by using attached properties from the AutomationProperties class, which in turn set native accessibility values. This article explains how to use the AutomationProperties class, so that a screen reader can speak about the elements on the page._

Xamarin.Forms allows automation properties to be set on user interface elements via the following attached properties:

- `AutomationProperties.IsInAccessibleTree` – indicates whether the element is available to an accessible application. For more information, see [AutomationProperties.IsInAccessibleTree](#automationpropertiesisinaccessibletree).
- `AutomationProperties.Name` – a short description of the element that serves as a speakable identifier for the element. For more information, see [AutomationProperties.Name](#automationpropertiesname).
- `AutomationProperties.HelpText` – a longer description of the element, which can be thought of as tooltip text associated with the element. For more information, see [AutomationProperties.HelpText](#automationpropertieshelptext).
- `AutomationProperties.LabeledBy` – allows another element to define accessibility information for the current element. For more information, see [AutomationProperties.LabeledBy](#automationpropertieslabeledby).

These attached properties set native accessibility values so that a screen reader can speak about the element. For more information about attached properties, see [Attached Properties](~/xamarin-forms/xaml/attached-properties.md).

> [!IMPORTANT]
> Using the `AutomationProperties` attached properties may impact UI Test execution on Android. The [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId), `AutomationProperties.Name` and `AutomationProperties.HelpText` properties will both set the native `ContentDescription` property, with the `AutomationProperties.Name` and `AutomationProperties.HelpText` property values taking precedence over the `AutomationId` value (if both `AutomationProperties.Name` and `AutomationProperties.HelpText` are set, the values will be concatenated). This means that any tests looking for `AutomationId` will fail if `AutomationProperties.Name` or `AutomationProperties.HelpText` are also set on the element. In this scenario, UI Tests should be altered to look for the value of `AutomationProperties.Name` or `AutomationProperties.HelpText`, or a concatenation of both.

Each platform has a different screen reader to narrate the accessibility values:

- iOS has VoiceOver. For more information, see [Test Accessibility on Your Device with VoiceOver](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) on developer.apple.com.
- Android has TalkBack. For more information, see [Testing Your App's Accessibility](https://developer.android.com/training/accessibility/testing.html#talkback) on developer.android.com.
- Windows has Narrator. For more information, see [Verify main app scenarios by using Narrator](/windows/uwp/accessibility/accessibility-testing#verify-main-app-scenarios-by-using-narrator).

However, the exact behavior of a screen reader depends on the software and on the user's configuration of it. For example, most screen readers read the text associated with a control when it receives focus, enabling users to orient themselves as they move among the controls on the page. Some screen readers also read the entire application user interface when a page appears, which enables the user to receive all of the page's available informational content before attempting to navigate it.

Screen readers also read different accessibility values. In the sample application:

- VoiceOver will read the `Placeholder` value of the `Entry`, followed by instructions for using the control.
- TalkBack will read the `Placeholder` value of the `Entry`, followed by the `AutomationProperties.HelpText` value, followed by instructions for using the control.
- Narrator will read the `AutomationProperties.LabeledBy` value of the `Entry`, followed by instructions on using the control.

In addition, Narrator will prioritize `AutomationProperties.Name`, `AutomationProperties.LabeledBy`, and then `AutomationProperties.HelpText`. On Android, TalkBack may combine the `AutomationProperties.Name` and `AutomationProperties.HelpText` values. Therefore, it's recommended that thorough accessibility testing is carried out on each platform to ensure an optimal experience.

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
> Note that the [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) method can also be used to set the `AutomationProperties.IsInAccessibleTree` attached property – `entry.SetValue(AutomationProperties.IsInAccessibleTreeProperty, true);`

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
> Note that the [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) method can also be used to set the `AutomationProperties.Name` attached property – `activityIndicator.SetValue(AutomationProperties.NameProperty, "Progress indicator");`

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
> Note that the [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) method can also be used to set the `AutomationProperties.HelpText` attached property – `button.SetValue(AutomationProperties.HelpTextProperty, "Tap to toggle the activity indicator");`

On some platforms, for edit controls such as an [`Entry`](xref:Xamarin.Forms.Entry), the `HelpText` property can sometimes be omitted and replaced with placeholder text. For example, "Enter your name here" is a good candidate for the [`Entry.Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) property that places the text in the control prior to the user's actual input.

## AutomationProperties.LabeledBy

The `AutomationProperties.LabeledBy` attached property allows another element to define accessibility information for the current element. For example, a [`Label`](xref:Xamarin.Forms.Label) next to an [`Entry`](xref:Xamarin.Forms.Entry) can be used to describe what the `Entry` represents. This can be accomplished in XAML as follows:

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

> [!IMPORTANT]
> The `AutomationProperties.LabeledByProperty` is not yet supported on iOS.

> [!NOTE]
> Note that the [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) method can also be used to set the `AutomationProperties.IsInAccessibleTree` attached property  – `entry.SetValue(AutomationProperties.LabeledByProperty, nameLabel);`

## Accessibility intricacies

The following sections describe the intricacies of setting accessibility values on certain controls.

### NavigationPage

On Android, to set the text that screen readers will read for the back arrow in the action bar in a [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), set the `AutomationProperties.Name` and `AutomationProperties.HelpText` properties on a [`Page`](xref:Xamarin.Forms.Page). However, note that this will not have an effect on OS back buttons.

### FlyoutPage

On iOS and the Universal Windows Platform (UWP), to set the text that screen readers will read for the toggle button on a [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage), either set the `AutomationProperties.Name`, and `AutomationProperties.HelpText` properties on the `FlyoutPage`, or on the `IconImageSource` property of the `Flyout` page.

On Android, to set the text that screen readers will read for the toggle button on a [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage), add string resources to the Android project:

```xml
<resources>
    <string name="app_name">Xamarin Forms Control Gallery</string>
    <string name="btnMDPAutomationID_open">Open Side Menu message</string>
    <string name="btnMDPAutomationID_close">Close Side Menu message</string>
</resources>
```

Then set the `AutomationId` property of the `IconImageSource` property of the `Flyout` page to the appropriate string:

```csharp
var flyout = new ContentPage { ... };
flyout.IconImageSource.AutomationId = "btnMDPAutomationID";
```

### ToolbarItem

On iOS, Android, and UWP, screen readers will read the `Text` property value of [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) instances, provided that `AutomationProperties.Name` or `AutomationProperties.HelpText` values aren't defined.

On iOS and UWP the `AutomationProperties.Name` property value will replace the `Text` property value that is read by the screen reader.

On Android, the `AutomationProperties.Name` and/or `AutomationProperties.HelpText` property values will completely replace the `Text` property value that is both visible and read by the screen reader. Note that this is a limitation of APIs less than 26.

## Related Links

- [Attached Properties](~/xamarin-forms/xaml/attached-properties.md)
- [Accessibility (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)