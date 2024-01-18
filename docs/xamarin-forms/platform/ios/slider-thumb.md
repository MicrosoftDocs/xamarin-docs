---
title: "Slider Thumb Tap on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that enables the Slider.Value property to be set by tapping on the Slider bar."
ms.service: xamarin
ms.assetid: D0915D37-9A59-4728-BB6A-FE094A661275
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Slider Thumb Tap on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This iOS platform-specific enables the [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) property to be set by tapping on a position on the [`Slider`](xref:Xamarin.Forms.Slider) bar, rather than by having to drag the `Slider` thumb. It's consumed in XAML by setting the [`Slider.UpdateOnTap`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) bindable property to `true`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

The `Slider.On<iOS>` method specifies that this platform-specific will only run on iOS. The [`Slider.SetUpdateOnTap`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.SetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider},System.Boolean)) method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, is used to control whether a tap on the `Slider` bar will set the [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) property. In addition, the [`Slider.GetUpdateOnTap`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.GetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider})) method can be used to return whether a tap on the `Slider` bar will set the `Slider.Value` property.

The result is that a tap on the [`Slider`](xref:Xamarin.Forms.Slider) bar can move the `Slider` thumb and set the [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) property:

![Slider Update on Tap enabled](slider-thumb-images/slider-updateontap.png)

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)