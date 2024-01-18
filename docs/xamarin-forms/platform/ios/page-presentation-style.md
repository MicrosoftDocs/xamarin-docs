---
title: "Modal Page Presentation Style on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that sets the presentation style of a modal page."
ms.service: xamarin
ms.assetid: C791F7CF-330A-44BA-987A-4CFCCBB9278B
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/02/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Modal Page Presentation Style on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This iOS platform-specific is used to set the presentation style of a modal page, and in addition can be used to display modal pages that have transparent backgrounds. It's consumed in XAML by setting the `Page.ModalPresentationStyle` bindable property to a `UIModalPresentationStyle` enumeration value:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.ModalPresentationStyle="OverFullScreen">
    ...
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSModalFormSheetPageCS : ContentPage
{
    public iOSModalFormSheetPageCS()
    {
        On<iOS>().SetModalPresentationStyle(UIModalPresentationStyle.OverFullScreen);
        ...
    }
}
```

The `Page.On<iOS>` method specifies that this platform-specific will only run on iOS. The `Page.SetModalPresentationStyle` method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, is used to set the modal presentation style on a [`Page`](xref:Xamarin.Forms.Page) by specifying one of the following `UIModalPresentationStyle` enumeration values:

- `FullScreen`, which sets the modal presentation style to encompass the whole screen. By default, modal pages are displayed using this presentation style.
- `FormSheet`, which sets the modal presentation style to be centered on and smaller than the screen.
- `Automatic`, which sets the modal presentation style to the default chosen by the system. For most view controllers, `UIKit` maps this to `UIModalPresentationStyle.PageSheet`, but some system view controllers may map it to a different style.
- `OverFullScreen`, which sets the modal presentation style to cover the screen.
- `PageSheet`, which sets the modal presentation style to cover the underlying content.

In addition, the `GetModalPresentationStyle` method can be used to retrieve the current value of the `UIModalPresentationStyle` enumeration that's applied to the [`Page`](xref:Xamarin.Forms.Page).

The result is that the modal presentation style on a [`Page`](xref:Xamarin.Forms.Page) can be set:

[![Modal Presentation Styles](page-presentation-style-images/modal-presentation-style-small.png)](page-presentation-style-images/modal-presentation-style-large.png#lightbox "Modal Presentation Styles")

> [!NOTE]
> Pages that use this platform-specific to set the modal presentation style must use modal navigation. For more information, see [Xamarin.Forms Modal Pages](~/xamarin-forms/app-fundamentals/navigation/modal.md).

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)