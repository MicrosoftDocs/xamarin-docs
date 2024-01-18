---
title: "VisualElement first responder on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that enables a VisualElement object to become the first responder to touch events."
ms.service: xamarin
ms.assetid: 3A77BA02-B87A-44EC-AC51-9D3130EF314C
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# VisualElement first responder on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This iOS platform-specific enables a [`VisualElement`](xref:Xamarin.Forms.VisualElement) object to become the first responder to touch events, rather than the page containing the element. It's consumed in XAML by setting the `VisualElement.CanBecomeFirstResponder` bindable property to `true`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry Placeholder="Enter text" />
        <Button ios:VisualElement.CanBecomeFirstResponder="True"
                Text="OK" />
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Entry entry = new Entry { Placeholder = "Enter text" };
Button button = new Button { Text = "OK" };
button.On<iOS>().SetCanBecomeFirstResponder(true);
```

The `VisualElement.On<iOS>` method specifies that this platform-specific will only run on iOS. The `VisualElement.SetCanBecomeFirstResponder` method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, is used to set the `VisualElement` to become the first responder for touch events. In addition, the `VisualElement.CanBecomeFirstResponder` method can be used to return whether the `VisualElement` is the first responder to touch events.

The result is that a [`VisualElement`](xref:Xamarin.Forms.VisualElement) can become the first responder for touch events, rather than the page containing the element. This enables scenarios such as chat applications not dismissing a keyboard when a [`Button`](xref:Xamarin.Forms.Button) is tapped.

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)