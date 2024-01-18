---
title: "WebView Zoom on Android"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the Android platform-specific that enables zoom on a WebView."
ms.service: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# WebView Zoom on Android

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This Android platform-specific enables pinch-to-zoom and a zoom control on a [`WebView`](xref:Xamarin.Forms.WebView). It's consumed in XAML by setting the `WebView.EnableZoomControls` and `WebView.DisplayZoomControls` bindable properties to `boolean` values:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

The `WebView.EnableZoomControls` bindable property controls whether pinch-to-zoom is enabled on the [`WebView`](xref:Xamarin.Forms.WebView), and the `WebView.DisplayZoomControls` bindable property controls whether zoom controls are overlaid on the `WebView`.

Alternatively, the platform-specific can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

The `WebView.On<Android>` method specifies that this platform-specific will only run on Android. The `WebView.EnableZoomControls` method, in the [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) namespace, is used to control whether pinch-to-zoom is enabled on the [`WebView`](xref:Xamarin.Forms.WebView). The `WebView.DisplayZoomControls` method, in the same namespace, is used to control whether zoom controls are overlaid on the `WebView`. In addition, the `WebView.ZoomControlsEnabled` and `WebView.ZoomControlsDisplayed` methods can be used to return whether pinch-to-zoom and zoom controls are enabled, respectively.

The result is that pinch-to-zoom can be enabled on a [`WebView`](xref:Xamarin.Forms.WebView), and zoom controls can be overlaid on the `WebView`:

[![Screenshot of zoomed WebView on Android](webview-zoom-controls-images/webview-zoom.png "Zoomed WebView")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "Zoomed WebView")

> [!IMPORTANT]
> Zoom controls must be both enabled and displayed, via the respective bindable properties or methods, to be overlaid on a [`WebView`](xref:Xamarin.Forms.WebView).

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)