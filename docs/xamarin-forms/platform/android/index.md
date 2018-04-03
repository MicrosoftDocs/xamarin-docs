---
title: "Android Platform Features"
description: "Adding Android-specific functionality to Xamarin.Forms apps"
ms.prod: xamarin
ms.assetid: E24168F3-0138-4814-86EA-B467F6B8A545
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/13/2016
---

# Android Platform Features

## Platform Support

The default Xamarin.Forms Android project uses an older style of control
renderering that was common prior to Android 5.0. Applications built using
the template have `FormsApplicationActivity` as the base class of their main
activity.

## Material Design via AppCompat

Xamarin.Forms also has an optional `FormsAppCompatActivity` that uses
**AppCompat** features provided by Android to implement Material Design
themes.

To add Material Design themes to your Xamarin.Forms Android project, follow the
[installation instructions for AppCompat support](appcompat.md)

Here is the **Todo** sample with the default `FormsApplicationActivity`:

[![](images/before-appcompat-sml.png "Todo Sample Application Without AppCompat")](images/before-appcompat.png#lightbox "Todo Sample Application Without AppCompat")

And this is the same code after upgrading the project to use `FormsAppCompatActivity`
(and adding the additional theme information):

[![](images/post-appcompat-sml.png "Todo Sample Application With AppCompat and Theming")](images/post-appcompat.png#lightbox "Todo Sample Application With AppCompat and Theming")

> [!NOTE]
> When using `FormsAppCompatActivity`, the [base classes for some Android custom renderers](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) will be different.


## Related Links

- [Add Material Design Support](appcompat.md)
