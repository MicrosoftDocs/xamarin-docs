---
title: "Accessibility Scaling for Named Font Sizes on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that disables accessibility scaling for named font sizes."
ms.service: xamarin
ms.assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Accessibility Scaling for Named Font Sizes on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This iOS platform-specific disables accessibility scaling for named font sizes. It's consumed in XAML by setting the `Application.EnableAccessibilityScalingForNamedFontSizes` bindable property to `false`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.EnableAccessibilityScalingForNamedFontSizes="false">
    ...
</Application>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetEnableAccessibilityScalingForNamedFontSizes(false);
```

The `Application.On<iOS>` method specifies that this platform-specific will only run on iOS. The `Application.SetEnableAccessibilityScalingForNamedFontSizes` method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, is used to disable named font sizes being scaled by the iOS accessibility settings. In addition, the `Application.GetEnableAccessibilityScalingForNamedFontSizes` method can be used to return whether named font sizes are scaled by iOS accessibility settings.

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)