---
title: "TimePicker item selection on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that controls when item selection occurs in a TimePicker."
ms.service: xamarin
ms.assetid: 554AC877-8698-4B8C-A676-80DD64305A06
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/15/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# TimePicker item selection on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This iOS platform-specific controls when item selection occurs in a [`TimePicker`](xref:Xamarin.Forms.TimePicker), allowing the user to specify that item selection occurs when browsing items in the control, or only once the **Done** button is pressed. It's consumed in XAML by setting the `TimePicker.UpdateMode` attached property to a value of the `UpdateMode` enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
       <TimePicker Time="14:00:00"
                   ios:TimePicker.UpdateMode="WhenFinished" />
       ...
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

The `TimePicker.On<iOS>` method specifies that this platform-specific will only run on iOS. The `TimePicker.SetUpdateMode` method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, is used to control when item selection occurs, with the `UpdateMode` enumeration providing two possible values:

- `Immediately` – item selection occurs as the user browses items in the [`TimePicker`](xref:Xamarin.Forms.TimePicker). This is the default behavior in Xamarin.Forms.
- `WhenFinished` – item selection only occurs once the user has pressed the **Done** button in the [`TimePicker`](xref:Xamarin.Forms.TimePicker).

In addition, the `SetUpdateMode` method can be used to toggle the enumeration values by calling the `UpdateMode` method, which returns the current `UpdateMode`:

```csharp
switch (timePicker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        timePicker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

The result is that a specified `UpdateMode` is applied to the [`TimePicker`](xref:Xamarin.Forms.TimePicker), which controls when item selection occurs:

[![Screenshot of TimePicker update modes](timepicker-selection-images/timepicker-updatemode.png "TimePicker UpdateMode Platform-Specific")](timepicker-selection-images/timepicker-updatemode-large.png#lightbox "TimePicker UpdateMode Platform-Specific")

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)