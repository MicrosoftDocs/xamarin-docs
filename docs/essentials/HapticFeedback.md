---
title: "Xamarin.Essentials: HapticFeedback"
description: "TThis document describes the HapticFeedback class in Xamarin.Essentials, which lets you perform the haptic feedback."
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: dimonovdd
ms.author: dimonovdd
ms.date: 18/08/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Essentials: HapticFeedback

The **HapticFeedback** class lets you perform the haptic feedback.

## Get started

[!include[](~/essentials/includes/get-started.md)]

To access the **HapticFeedback** functionality the following platform specific setup is required.

# [Android](#tab/android)

No additional setup required.

# [iOS](#tab/ios)

No additional setup required.

# [UWP](#tab/uwp)

No additional setup required.

# [MacOs](#tab/macos)

No additional setup required.

# [Tizen](#tab/tizen)

No additional setup required.
-----

## Using HapticFeedback

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The Haptic Feedback can be performed for "LongPress" type or the default "Click".

```csharp
try
{
    // Use default vibration length
    HapticFeedback.Perform();

    // Or use specified time
    HapticFeedback.Perform(HapticFeedbackType.LongPress);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```


## Platform Differences

# [Android](#tab/android)
- Click - FeedbackConstants.ContextClick
- LongPress - FeedbackConstants.LongPress

# [iOS](#tab/ios)
- Click - UIImpactFeedbackStyle.Light
- LongPress - UIImpactFeedbackStyle.Medium

# [UWP](#tab/uwp)
- Click - KnownSimpleHapticsControllerWaveforms.Click
- LongPress - KnownSimpleHapticsControllerWaveforms.Press

# [MacOs](#tab/macos)
- LongPress - NSHapticFeedbackPattern.Generic

# [Tizen](#tab/tizen)
- Click - Tap
- LongPress - Hold

-----

## API

- [HapticFeedback source code](https://github.com/xamarin/Essentials/tree/develop/Xamarin.Essentials/HapticFeedback)
- [HapticFeedback API documentation](xref:Xamarin.Essentials.HapticFeedback)


