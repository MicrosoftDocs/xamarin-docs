---
title: "Entry Cursor Color on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that sets the cursor color of an Entry."
ms.service: xamarin
ms.assetid: 867D70BA-53F9-4434-8094-85D71DCECC2D
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Entry Cursor Color on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This iOS platform-specific sets the cursor color of an [`Entry`](xref:Xamarin.Forms.Entry) to a specified color. It's consumed in XAML by setting the [`Entry.CursorColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.CursorColorProperty) bindable property to a [`Color`](xref:Xamarin.Forms.Color):

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Entry ... ios:Entry.CursorColor="LimeGreen" />
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var entry = new Xamarin.Forms.Entry();
entry.On<iOS>().SetCursorColor(Color.LimeGreen);
```

The `Entry.On<iOS>` method specifies that this platform-specific will only run on iOS. The [`Entry.SetCursorColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetCursorColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry},Xamarin.Forms.Color)) method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, sets the cursor color to a specified [`Color`](xref:Xamarin.Forms.Color). In addition, the [`Entry.GetCursorColor`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.GetCursorColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry})) method can be used to retrieve the current cursor color.

The result is that the cursor color in a [`Entry`](xref:Xamarin.Forms.Entry) can be set to a specific [`Color`](xref:Xamarin.Forms.Color):

![Entry Cursor Color](entry-cursor-color-images/entry-cursorcolor.png)

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)