---
title: "Page Status Bar Visibility on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that sets the visibility of the status bar on a Page."
ms.service: xamarin
ms.assetid: D8BB7C24-A27F-4758-8557-6A81F909ABD9
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Page Status Bar Visibility on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This iOS platform-specific is used to set the visibility of the status bar on a [`Page`](xref:Xamarin.Forms.Page), and it includes the ability to control how the status bar enters or leaves the `Page`. It's consumed in XAML by setting the `Page.PrefersStatusBarHidden` attached property to a value of the `StatusBarHiddenMode` enumeration, and optionally the `Page.PreferredStatusBarUpdateAnimation` attached property to a value of the `UIStatusBarAnimation` enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

The `Page.On<iOS>` method specifies that this platform-specific will only run on iOS. The `Page.SetPrefersStatusBarHidden` method, in the `Xamarin.Forms.PlatformConfiguration.iOSSpecific` namespace, is used to set the visibility of the status bar on a [`Page`](xref:Xamarin.Forms.Page) by specifying one of the `StatusBarHiddenMode` enumeration values: `Default`, `True`, or `False`. The `StatusBarHiddenMode.True` and `StatusBarHiddenMode.False` values set the status bar visibility regardless of device orientation, and the `StatusBarHiddenMode.Default` value hides the status bar in a vertically compact environment.

The result is that the visibility of the status bar on a [`Page`](xref:Xamarin.Forms.Page) can be set:

![Status Bar Visibility Platform-Specific](page-status-bar-visibility-images/hide-status-bar.png)

> [!NOTE]
> On a [`TabbedPage`](xref:Xamarin.Forms.TabbedPage), the specified `StatusBarHiddenMode` enumeration value will also update the status bar on all child pages. On all other [`Page`](xref:Xamarin.Forms.Page)-derived types, the specified `StatusBarHiddenMode` enumeration value will only update the status bar on the current page.

The `Page.SetPreferredStatusBarUpdateAnimation` method is used to set how the status bar enters or leaves the [`Page`](xref:Xamarin.Forms.Page) by specifying one of the `UIStatusBarAnimation` enumeration values: `None`, `Fade`, or `Slide`. If the `Fade` or `Slide` enumeration value is specified, a 0.25 second animation executes as the status bar enters or leaves the `Page`.

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)