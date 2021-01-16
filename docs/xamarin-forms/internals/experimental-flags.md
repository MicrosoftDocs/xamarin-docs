---
title: "Xamarin.Forms experimental flags"
description: "Xamarin.Forms experimental flags enable the engineering team to ship new features to users more quickly, while still being able to change feature APIs before they move to a stable release."
ms.prod: xamarin
ms.assetid: AF4BDD27-89F6-48AE-A8CD-D7E4DDA2CCA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms experimental flags

When a new Xamarin.Forms feature is implemented, it's sometimes put behind an experimental flag. This enables the engineering team to provide new features to you more quickly, while still being able to change feature APIs before they move to a stable release. The experimental flag is then removed once the feature moves to a stable release.

Xamarin.Forms includes the following experimental flags:

- `Shell_UWP_Experimental`

Using functionality that's behind an experimental flag requires you to enable the flag, or flags, in your application. There are two approaches for enabling experimental flags:

- Enable the experimental flag in your platform projects.
- Enable the experimental flag in your `App` class.

> [!WARNING]
> Consuming functionality that's behind an experimental flag, without enabling the flag, will result in your application throwing an exception that indicates which flag must be enabled.

## Enable flags in platform projects

The `Xamarin.Forms.Forms.SetFlags` method can be used to enable an experimental flag in your platform projects:

```csharp
Xamarin.Forms.Forms.SetFlags("Shell_UWP_Experimental");
```

The `SetFlags` method should be invoked in your `AppDelegate` class on iOS, in your `MainActivity` class on Android, and in your `App` class on UWP.

> [!IMPORTANT]
> Enabling an experimental flag in your platform projects must occur before the `Forms.Init` method is invoked.

The `Xamarin.Forms.Forms.SetFlags` method accepts a `string` array argument, which makes it possible to enable multiple experimental flags in a single method call:

```csharp
Xamarin.Forms.Forms.SetFlags(new string[] { "Shell_UWP_Experimental", "AnotherFeature_Experimental" });
```

> [!WARNING]
> Never call the `SetFlags` method more than once, as subsequent calls will overwrite the result of previous calls.

## Enable flags in your App class

The `Device.SetFlags` method can be used to enable an experimental flag in the `App` class in your shared code project:

```csharp
Device.SetFlags(new string[]{ "Shell_UWP_Experimental" });
```

The `Device.SetFlags` method accepts an `IReadOnlyList<string>` argument, which makes it possible to enable multiple experimental flags in a single method call:

```csharp
Device.SetFlags(new string[]{ "Shell_UWP_Experimental", "AnotherFeature_Experimental" });
```

> [!WARNING]
> Never call the `SetFlags` method more than once, as subsequent calls will overwrite the result of previous calls.

## Old experimental flags

The following table lists experimental flags for features that are now in general availability, and the Xamarin.Forms release in which the experimental flag was removed:

| Flag | Xamarin.Forms Release |
| ---- | --------------------- |
| `AppTheme_Experimental` | 4.8 |
| `Brush_Experimental` | 5.0 |
| `CarouselView_Experimental` | 5.0 |
| `CollectionView_Experimental` | 4.3 |
| `DragAndDrop_Experimental` | 5.0 |
| `FastRenderers_Experimental` | 4.0 |
| `IndicatorView_Experimental` | 4.7 |
| `Markup_Experimental` | 5.0 (moved to Xamarin Community Toolkit) |
| `MediaElement_Experimental` | 5.0 (moved to Xamarin Community Toolkit) |
| `RadioButton_Experimental` | 5.0 |
| `Shapes_Experimental` | 5.0 |
| `Shell_Experimental` | 4.0  |
| `StateTriggers_Experimental` | 4.7 |
| `SwipeView_Experimental` | 5.0 |
| `Visual_Experimental` | 3.6 |
