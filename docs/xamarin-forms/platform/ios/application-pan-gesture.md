---
title: "Simultaneous Pan Gesture Recognition on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that enables simultaneous pan gesture recognition to be used in an application."
ms.service: xamarin
ms.assetid: 883D89DA-F8FF-4B97-9C3F-2DD05C96A495
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Simultaneous Pan Gesture Recognition on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

When a [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) is attached to a view inside a scrolling view, all of the pan gestures are captured by the `PanGestureRecognizer` and aren't passed to the scrolling view. Therefore, the scrolling view will no longer scroll.

This iOS platform-specific enables a `PanGestureRecognizer` in a scrolling view to capture and share the pan gesture with the scrolling view. It's consumed in XAML by setting the [`Application.PanGestureRecognizerShouldRecognizeSimultaneously`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.PanGestureRecognizerShouldRecognizeSimultaneouslyProperty) attached property to `true`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.PanGestureRecognizerShouldRecognizeSimultaneously="true">
    ...
</Application>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetPanGestureRecognizerShouldRecognizeSimultaneously(true);
```

The `Application.On<iOS>` method specifies that this platform-specific will only run on iOS. The [`Application.SetPanGestureRecognizerShouldRecognizeSimultaneously`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.SetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application},System.Boolean)) method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, is used to control whether a pan gesture recognizer in a scrolling view will capture the pan gesture, or capture and share the pan gesture with the scrolling view. In addition, the [`Application.GetPanGestureRecognizerShouldRecognizeSimultaneously`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Application.GetPanGestureRecognizerShouldRecognizeSimultaneously(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Application})) method can be used to return whether the pan gesture is shared with the scrolling view that contains the [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer).

Therefore, with this platform-specific enabled, when a [`ListView`](xref:Xamarin.Forms.ListView) contains a [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer), both the `ListView` and the `PanGestureRecognizer` will receive the pan gesture and process it. However, with this platform-specific disabled, when a `ListView` contains a `PanGestureRecognizer`, the `PanGestureRecognizer` will capture the pan gesture and process it, and the `ListView` won't receive the pan gesture.

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)