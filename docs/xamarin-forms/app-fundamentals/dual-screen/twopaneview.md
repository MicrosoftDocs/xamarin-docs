---
title: "Xamarin.Forms TwoPaneView"
description: "This guide explains how to use Xamarin.Forms TwoPaneView to optimize your app experience for dual-screen devices such as Surface Duo and Surface Neo."
ms.prod: xamarin
ms.assetid: 17ee8afa-5e7c-4a4f-a9b6-2aca03f30fe3
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
---

# Xamarin.Forms TwoPaneView

Represents a container with two views that size and position content in the available space, either side-by-side or top-bottom.

## Setting up TwoPaneView

The `TwoPaneView` property `Source` can take a URI or local file path. Playback will begin immediately upon the media opening.

```xaml
<ContentPage xmlns:dualScreen="clr-namespace:Xamarin.Forms.DualScreen;assembly=Xamarin.Forms.DualScreen">
    <dualScreen:TwoPaneView>
        <dualScreen:TwoPaneView.Pane1>
            <StackLayout>
                <Label Text="Pane1 Content"></Label>
            </StackLayout>
        </dualScreen:TwoPaneView.Pane1>
        <dualScreen:TwoPaneView.Pane2>
            <StackLayout>
                <Label Text="Pane2 Content"></Label>
            </StackLayout>
        </dualScreen:TwoPaneView.Pane2>
    </dualScreen:TwoPaneView>
</ContentPage>
```
