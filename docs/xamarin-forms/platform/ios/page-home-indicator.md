---
title: "Home Indicator Visibility on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that sets the visibility of the home indicator on a Page."
ms.service: xamarin
ms.assetid: F81022E0-3C6C-49C0-A000-FAF6574D3FB7
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Home Indicator Visibility on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This iOS platform-specific sets the visibility of the home indicator on a [`Page`](xref:Xamarin.Forms.Page). It's consumed in XAML by setting the [`Page.PrefersHomeIndicatorAutoHidden`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHiddenProperty) bindable property to a `boolean`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersHomeIndicatorAutoHidden="true">
    ...
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersHomeIndicatorAutoHidden(true);
```

The `Page.On<iOS>` method specifies that this platform-specific will only run on iOS. The [`Page.SetPrefersHomeIndicatorAutoHidden`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.SetPrefersHomeIndicatorAutoHidden(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Page},System.Boolean)) method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, controls the visibility of the home indicator. In addition, the [`Page.PrefersHomeIndicatorAutoHidden`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHidden(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Page})) method can be used to retrieve the visibility of the home indicator.

The result is that the visibility of the home indicator on a [`Page`](xref:Xamarin.Forms.Page) can be controlled:

![Screenshot of home indicator visibility on an iOS page](page-home-indicator-images/home-indicator-visibility.png "Page home indicator visibility")

> [!NOTE]
> This platform-specific can be applied to [`ContentPage`](xref:Xamarin.Forms.ContentPage), [`FlyoutPage`](xref:Xamarin.Forms.FlyoutPage), [`NavigationPage`](xref:Xamarin.Forms.NavigationPage), and [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) objects.

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)