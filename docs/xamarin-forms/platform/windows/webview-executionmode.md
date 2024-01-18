---
title: "WebView Execution Mode on Windows"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the Windows platform-specific that sets the thread on which a WebView hosts its content."
ms.service: xamarin
ms.assetid: 4D3C4112-0FF6-47F8-BC22-579D92032807
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/10/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# WebView Execution Mode on Windows

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This platform-specific sets the thread on which a [`WebView`](xref:Xamarin.Forms.WebView) hosts its content. It's consumed in XAML by setting the `WebView.ExecutionMode` bindable property to a `WebViewExecutionMode` enumeration value:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.ExecutionMode="SeparateThread" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

WebView webView = new Xamarin.Forms.WebView();
webView.On<Windows>().SetExecutionMode(WebViewExecutionMode.SeparateThread);
```

The `WebView.On<Windows>` method specifies that this platform-specific will only run on the Universal Windows Platform. The `WebView.SetExecutionMode` method, in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace, is used to set the thread on which a `WebView` hosts its content, with the `WebViewExecutionMode` enumeration providing three possible values:

- `SameThread` indicates that content is hosted on the UI thread. This is the default value for the `WebView` on Windows.
- `SeparateThread` indicates that content is hosted on a background thread.
- `SeparateProcess` indicates that content is hosted on a separate process off the app process. There isn't a separate process per WebView instance, and so all of an app's WebView instances share the same separate process.

In addition, the `GetExecutionMode` method can be used to return the current `WebViewExecutionMode` for the `WebView`.

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [WindowsSpecific API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
