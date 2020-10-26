---
title: "Xamarin.Forms Dual-Screen"
description: "This guide explains how to create Xamarin.Forms apps for dual-screen devices."
ms.prod: xamarin
ms.assetid: f9906e83-f8ae-48f9-997b-e1540b96ee8e
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms dual-screen

Dual-screen devices like the Microsoft Surface Duo facilitate new user-experience possibilities for your applications. Xamarin.Forms includes `TwoPaneView` and `DualScreenInfo` classes so you can develop apps for dual-screen devices.

## Get started

Follow these steps to add dual-screen capabilities to a Xamarin.Forms app:

1. Open the **NuGet Package Manager** dialog for your solution.
2. Under the **Browse** tab, search for `Xamarin.Forms.DualScreen`.
3. Install the `Xamarin.Forms.DualScreen` package to your solution.
4. Add the following initialization method call to the Android project's `MainActivity` class, in the `OnCreate` event:

    ```csharp
    Xamarin.Forms.DualScreen.DualScreenService.Init(this);
    ```

    This method is required for the app to be able to detect changes in the app's state, such as being spanned across two screens.

5. Update the `Activity` attribute on the Android project's `MainActivity` class, so that it includes _all_ these `ConfigurationChanges` options:

    ```@csharp
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation
        | ConfigChanges.ScreenLayout | ConfigChanges.SmallestScreenSize | ConfigChanges.UiMode
    ```

    These values are required so that configuration changes and span state can be more reliably reported. By default only two are added to Xamarin.Forms projects, so remember to add the rest for reliable dual-screen support.

## Troubleshooting

If the `DualScreenInfo` class or `TwoPaneView` layout aren't working as expected, double-check the set-up instructions on this page. Omitting or misconfiguring the `Init` method or the `ConfigurationChanges` attribute values are common causes of errors.

Review the [Xamarin.Forms dual-screen samples](/dual-screen/xamarin/samples) for additional guidance and reference implementation.

## Next steps

Once you've added the NuGet, add dual-screen features to your app with the following guidance:

- [Dual-screen design patterns](design-patterns.md) - When considering how to best utilize multiple screens on a dual-screen device, refer to this pattern guidance to find the best fit for your application interface.
- [TwoPaneView layout](twopaneview.md) - The Xamarin.Forms `TwoPaneView` class, inspired by the UWP control of the same name, is a cross-platform layout optimized for dual-screen devices.
- [DualScreenInfo helper class](dual-screen-info.md) - The `DualScreenInfo` class enables you to determine which pane your view is on, how big it is, what posture the device is in, the angle of the hinge, and more.
- [Dual-screen triggers](triggers.md) - The [`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen) namespace includes two state triggers that trigger a [`VisualState`](xref:Xamarin.Forms.VisualState) change when the view mode of the attached layout, or window, changes.

Visit the [dual-screen developer docs](/dual-screen/) for more information.