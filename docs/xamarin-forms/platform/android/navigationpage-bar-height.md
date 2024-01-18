---
title: "NavigationPage Bar Height on Android"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the Android platform-specific that sets the height of the navigation bar on a NavigationPage."
ms.service: xamarin
ms.assetid: C8A73B64-FE70-408A-A72E-8AF147F0C52C
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# NavigationPage Bar Height on Android

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This Android platform-specific sets the height of the navigation bar on a [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). It's consumed in XAML by setting the [`NavigationPage.BarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) bindable property to an integer value:

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

public class AndroidNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public AndroidNavigationPageCS()
    {
        On<Android>().SetBarHeight(450);
    }
}
```

The `NavigationPage.On<Android>` method specifies that this platform-specific will only run on app compat Android. The [`NavigationPage.SetBarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.SetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage},System.Int32)) method, in the [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) namespace, is used to set the height of the navigation bar on a [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). In addition, the [`NavigationPage.GetBarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.GetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage})) method can be used to return the height of the navigation bar in the `NavigationPage`.

The result is that the height of the navigation bar on a [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) can be set:

![NavigationPage navigation bar height](navigationpage-bar-height-images/navigationpage-barheight.png)

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)