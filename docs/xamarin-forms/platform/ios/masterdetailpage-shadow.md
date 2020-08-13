---
title: "MasterDetailPage Shadow on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that controls whether the detail page of a MasterDetailPage has shadow applied to it, when revealing the master page."
ms.prod: xamarin
ms.assetid: FB907EA2-C00A-43A5-84B1-A43584C867A5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# MasterDetailPage Shadow on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This platform-specific controls whether the detail page of a [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) has shadow applied to it, when revealing the master page. It's consumed in XAML by setting the `MasterDetailPage.ApplyShadow` bindable property to `true`:

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                  ios:MasterDetailPage.ApplyShadow="true">
    ...
</MasterDetailPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSMasterDetailPageCS : MasterDetailPage
{
    public iOSMasterDetailPageCS(ICommand restore)
    {
        On<iOS>().SetApplyShadow(true);
        // ...
    }
}
```

The `MasterDetailPage.On<iOS>` method specifies that this platform-specific will only run on iOS. The `MasterDetailPage.SetApplyShadow` method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, is used to control whether the detail page of a [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) has shadow applied to it, when revealing the master page. In addition, the `GetApplyShadow` method can be used to determine whether shadow is applied to the detail page of a `MasterDetailPage`.

The result is that the detail page of a [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) can have shadow applied to it, when revealing the master page:

[![Screenshot of a MasterDetailPage with and without shadow](masterdetailpage-shadow-images/shadow.png "MasterDetailPage with and without shadow")](masterdetailpage-shadow-images/shadow-large.png#lightbox "MasterDetailPage with and without shadow")

## Related links

- [PlatformSpecifics (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
