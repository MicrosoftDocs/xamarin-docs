---
title: "Xamarin.Forms dual-screen layout"
description: "This guide explains how to use Xamarin.Forms TwoPaneView to optimize your app experience for dual-screen devices such as Surface Duo and Surface Neo."
ms.prod: xamarin
ms.assetid: 17ee8afa-5e7c-4a4f-a9b6-2aca03f30fe3
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
---

# Xamarin.Forms dual-screen layout

![](~/media/shared/preview.png "This API is currently pre-release")

[![Download Sample](~/media/shared/download.png) Download the sample](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/DualScreenDemos)

The `TwoPaneView` class represents a container with two views that size and position content in the available space, either side-by-side or top-to-bottom. `TwoPaneView` inherits from `Grid` so the easiest way to think about these properties is as if they are being applied to a grid.

## Set up TwoPaneView

The `TwoPaneView.Source` property can take a URI or local file path. Playback will begin immediately upon the media opening:

```xaml
<ContentPage xmlns:dualScreen="clr-namespace:Xamarin.Forms.DualScreen;assembly=Xamarin.Forms.DualScreen">
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

- `TallModeConfiguration` indicates, when in tall mode, the Left/Right arrangement or if you only want a single pane visible as defined by the TwoPaneViewPriority.
- `WideModeConfiguration` indicates, when in wide mode, the Top/Bottom arrangement or if you only want a single pane visible as defined by the TwoPaneViewPriority.
- `PanePriority` determines whether to show Pane1 or Pane2 if in SinglePane mode.

## Related links

- [DualScreen (sample)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/DualScreenDemos)
