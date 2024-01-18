---
title: "ImageButton Drop Shadows on Android"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the Android platform-specific that enables a drop shadow on a ImageButton."
ms.service: xamarin
ms.assetid: D3604D87-9F9F-4FE2-8B10-DF3B143C0734
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# ImageButton Drop Shadows on Android

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This Android platform-specific is used to enable a drop shadow on a `ImageButton`. It's consumed in XAML by setting the `ImageButton.IsShadowEnabled` bindable property to `true`, along with a number of additional optional bindable properties that control the drop shadow:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
       <ImageButton ...
                    Source="XamarinLogo.png"
                    BackgroundColor="GhostWhite"
                    android:ImageButton.IsShadowEnabled="true"
                    android:ImageButton.ShadowColor="Gray"
                    android:ImageButton.ShadowRadius="12">
            <android:ImageButton.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </android:ImageButton.ShadowOffset>
        </ImageButton>
        ...
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var imageButton = new Xamarin.Forms.ImageButton { Source = "XamarinLogo.png", BackgroundColor = Color.GhostWhite, ... };
imageButton.On<Android>()
           .SetIsShadowEnabled(true)
           .SetShadowColor(Color.Gray)
           .SetShadowOffset(new Size(10, 10))
           .SetShadowRadius(12);
```

> [!IMPORTANT]
> A drop shadow is drawn as part of the `ImageButton` background, and the background is only drawn if the `BackgroundColor` property is set. Therefore, a drop shadow will not be drawn if the `ImageButton.BackgroundColor` property isn't set.

The `ImageButton.On<Android>` method specifies that this platform-specific will only run on Android. The `ImageButton.SetIsShadowEnabled` method, in the [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) namespace, is used to control whether a drop shadow is enabled on the `ImageButton`. In addition, the following methods can be invoked to control the drop shadow:

- `SetShadowColor` – sets the color of the drop shadow. The default color is [`Color.Default`](xref:Xamarin.Forms.Color.Default*).
- `SetShadowOffset` – sets the offset of the drop shadow. The offset changes the direction the shadow is cast, and is specified as a [`Size`](xref:Xamarin.Forms.Size) value. The `Size` structure values are expressed in device-independent units, with the first value being the distance to the left (negative value) or right (positive value), and the second value being the distance above (negative value) or below (positive value). The default value of this property is (0.0, 0.0), which results in the shadow being cast around every side of the `ImageButton`.
- `SetShadowRadius`– sets the blur radius used to render the drop shadow. The default radius value is 10.0.

> [!NOTE]
> The state of a drop shadow can be queried by calling the `GetIsShadowEnabled`, `GetShadowColor`, `GetShadowOffset`, and `GetShadowRadius` methods.

The result is that a drop shadow can be enabled on a `ImageButton`:

![ImageButton with drop shadow](imagebutton-drop-shadow-images/imagebutton-drop-shadow.png)

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)