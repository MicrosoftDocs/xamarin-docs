---
title: "Xamarin.Forms dual-screen platform helpers"
description: "This guide explains how to use Xamarin.Forms DualScreenHelper class to optimize your app experience for dual-screen devices such as Surface Duo and Surface Neo."
ms.prod: xamarin
ms.assetid: 5aa184c2-5611-427d-85c7-1c56486c3e1b
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/08/2020
---

# Xamarin.Forms dual-screen platform helpers

[![Download Sample](~/media/shared/download.png) Download the sample](https://github.com/xamarin/xamarin-forms-samples/UserInterface/DualScreenDemos)

The `DualScreenHelper` class can be used to detect if a device supports Picture in Picture mode and then lets you open a `ContentPage` as a picture in picture window. If you're running on a Neo device, it will open the page into the WonderBar when in compose mode.

## Properties

- `HasCompactModeSupport` is used to check if the platform supports opening a `ContentPage` in CompactMode.
- `OpenCompactMode` opens a `ContentPage` in CompactMode, if supported by the platform.

## Example

```csharp
async void OpenCompactWindowClicked(object sender, EventArgs e)
{
    if (!DualScreenHelper.HasCompactModeSupport())
    {
        await DisplayAlert("Unsupported", "This platform doesn't support this feature", "Ok");
        return;
    }

    ContentPage page = new ContentPage() { BackgroundColor = Color.Purple };

    var button = new Button()
    {
        Text = "Close this Window"
    };

    var layout = new StackLayout()
    {
        Children =
        {
            new Label(){ Text = "Welcome to your Compact Mode Window" },
            button
        },
        BackgroundColor = Color.Yellow,
        HorizontalOptions = LayoutOptions.Fill,
        VerticalOptions = LayoutOptions.Fill
    };

    page.Content = new ScrollView() { Content = layout };
    var args = await DualScreenHelper.OpenCompactMode(page);

    button.Command = new Command(async () =>
    {
        await args.CloseAsync();
    });
}
```
