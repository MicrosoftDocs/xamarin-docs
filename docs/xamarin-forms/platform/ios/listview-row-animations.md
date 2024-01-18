---
title: "ListView Row Animations on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that controls whether row animations are disabled when the ListView items collection is being updated."
ms.service: xamarin
ms.assetid: E8F5103F-4D8E-4A5A-A16C-7FA14EE786AC
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# ListView Row Animations on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This iOS platform-specific controls whether row animations are disabled when the [`ListView`](xref:Xamarin.Forms.ListView) items collection is being updated. It's consumed in XAML by setting the `ListView.RowAnimationsEnabled` bindable property to `false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.RowAnimationsEnabled="false">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetRowAnimationsEnabled(false);
```

The `ListView.On<iOS>` method specifies that this platform-specific will only run on iOS. The `ListView.SetRowAnimationsEnabled` method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, is used to control whether row animations are disabled when the [`ListView`](xref:Xamarin.Forms.ListView) items collection is being updated. In addition, the `ListView.GetRowAnimationsEnabled` method can be used to return whether row animations are disabled on the `ListView`.

> [!NOTE]
> [`ListView`](xref:Xamarin.Forms.ListView) row animations are enabled by default. Therefore, an animation occurs when a new row is inserted into a `ListView`.

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)