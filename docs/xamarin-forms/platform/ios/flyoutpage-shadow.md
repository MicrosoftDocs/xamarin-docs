---
title: "FlyoutPage Shadow on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that controls whether the detail page of a FlyoutPage has shadow applied to it, when revealing the flyout page."
ms.service: xamarin
ms.assetid: FB907EA2-C00A-43A5-84B1-A43584C867A5
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# FlyoutPage Shadow on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This platform-specific controls whether the detail page of a [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) has shadow applied to it, when revealing the flyout page. It's consumed in XAML by setting the `FlyoutPage.ApplyShadow` bindable property to `true`:

```xaml
<FlyoutPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                  ios:FlyoutPage.ApplyShadow="true">
    ...
</FlyoutPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSFlyoutPageCS : FlyoutPage
{
    public iOSFlyoutPageCS(ICommand restore)
    {
        On<iOS>().SetApplyShadow(true);
        // ...
    }
}
```

The `FlyoutPage.On<iOS>` method specifies that this platform-specific will only run on iOS. The `FlyoutPage.SetApplyShadow` method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, is used to control whether the detail page of a [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) has shadow applied to it, when revealing the flyout page. In addition, the `GetApplyShadow` method can be used to determine whether shadow is applied to the detail page of a `FlyoutPage`.

The result is that the detail page of a [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage) can have shadow applied to it, when revealing the flyout page:

[![Screenshot of a FlyoutPage with and without shadow](flyoutpage-shadow-images/shadow.png "FlyoutPage with and without shadow")](flyoutpage-shadow-images/shadow-large.png#lightbox "FlyoutPage with and without shadow")

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)