---
title: "Xamarin.Forms dual-screen layout"
description: "This guide explains how to use Xamarin.Forms TwoPaneView to optimize your app experience for dual-screen devices such as Surface Duo and Surface Neo."
ms.service: xamarin
ms.assetid: 17ee8afa-5e7c-4a4f-a9b6-2aca03f30fe3
ms.subservice: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms TwoPaneView layout

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)

The `TwoPaneView` class represents a container with two views that size and position content in the available space, either side-by-side or top-to-bottom. `TwoPaneView` inherits from `Grid` so the easiest way to think about these properties is as if they are being applied to a grid.

## Set up TwoPaneView

Follow these instructions to create a dual-screen layout in your app:

1. Follow the [get started](index.md) instructions to add the NuGet and configure the Android `MainActivity` class.
1. Start with a basic `TwoPaneView` using the following XAML:

    ```xaml
    <ContentPage
        xmlns:dualScreen="clr-namespace:Xamarin.Forms.DualScreen;assembly=Xamarin.Forms.DualScreen">
        <dualScreen:TwoPaneView>
            <dualScreen:TwoPaneView.Pane1>
                <StackLayout>
                    <Label Text="Pane1 Content" />
                </StackLayout>
            </dualScreen:TwoPaneView.Pane1>
            <dualScreen:TwoPaneView.Pane2>
                <StackLayout>
                    <Label Text="Pane2 Content" />
                </StackLayout>
            </dualScreen:TwoPaneView.Pane2>
        </dualScreen:TwoPaneView>
    </ContentPage>
    ```

> [!TIP]
> The above XAML omits many common attributes from the `ContentPage` element. When adding a `TwoPaneView` to your app, remember to declare the `xmlns:dualScreen` namespace as shown.

## Understand TwoPaneView modes

Only one of these modes can be active:

- `SinglePane` only one pane is currently visible.
- `Wide` the two panes are laid out horizontally. One pane is on the left and the other is on the right. When on two screens this is the mode when the device is portrait.
- `Tall` the two panes are laid out vertically. One pane is on top and the other is on bottom. When on two screens this is the mode when the device is landscape.

## Control TwoPaneView when it's only on one screen

The following properties apply when the `TwoPaneView` is occupying a single screen:

- `MinTallModeHeight` indicates the minimum height the control must be to enter tall mode.
- `MinWideModeWidth` indicates the minimum width the control must be to enter wide mode.
- `Pane1Length` sets the width of Pane1 in Wide mode, the height of Pane1 in Tall mode, and has no effect in SinglePane mode.
- `Pane2Length` sets the width of Pane2 in Wide mode, the height of Pane2 in Tall mode, and has no effect in SinglePane mode.

> [!IMPORTANT]
> If the `TwoPaneView` is spanned across two screens these properties have no effect.

## Properties that apply when on one screen or two

The following properties apply when the `TwoPaneView` is occupying a single screen or two screens:

- `TallModeConfiguration` indicates, when in tall mode, the Top/Bottom arrangement or if you only want a single pane visible as defined by the TwoPaneViewPriority.
- `WideModeConfiguration` indicates, when in wide mode, the Left/Right arrangement or if you only want a single pane visible as defined by the TwoPaneViewPriority.
- `PanePriority` determines whether to show Pane1 or Pane2 if in SinglePane mode.

## Related links

- [DualScreen (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-dualscreendemos/)