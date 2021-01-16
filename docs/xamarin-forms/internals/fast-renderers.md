---
title: "Xamarin.Forms Fast Renderers"
description: "This article introduces fast renderers, which reduce the inflation and rendering costs of a Xamarin.Forms control on Android by flattening the resulting native control hierarchy."
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Fast Renderers

Traditionally, most of the original control renderers on Android are composed of two views:

- A native control, such as a `Button` or `TextView`.
- A container `ViewGroup` that handles some of the layout work, gesture handling, and other tasks.

However, this approach has a performance implication in that two views are created for each logical control, which results in a more complex visual tree that requires more memory, and more processing to render on screen.

Fast renderers reduce the inflation and rendering costs of a Xamarin.Forms control into a single view. Therefore, instead of creating two views and adding them to the view tree, only one is created. This improves performance by creating fewer objects, which in turn means a less complex view tree, and less memory use (which also results in fewer garbage collection pauses).

Fast renderers are available for the following controls in Xamarin.Forms on Android:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)

Functionally, these fast renderers are no different to the legacy renderers. From Xamarin.Forms 4.0 onwards, all applications targeting `FormsAppCompatActivity` will use these fast renderers by default. Renderers for all new controls, including [`ImageButton`](xref:Xamarin.Forms.ImageButton) and [`CollectionView`](xref:Xamarin.Forms.CollectionView), use the fast renderer approach.

Performance improvements when using fast renderers will vary for each application, depending upon the complexity of the layout. For example, performance improvements of x2 are possible when scrolling through a [`ListView`](xref:Xamarin.Forms.ListView) containing thousands of rows of data, where the cells in each row are made of controls that use fast renderers, which results in visibly smoother scrolling.

> [!NOTE]
> Custom renderers can be created for fast renderers using the same approach as used for the legacy renderers. For more information, see [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## Backwards compatibility

Fast renderers can be overridden with the following approaches:

1. Enabling the legacy renderers by adding the following line of code to your `MainActivity` class before calling `Forms.Init`:

    ```csharp
    Forms.SetFlags("UseLegacyRenderers");
    ```

1. Using custom renderers that target the legacy renderers. Any existing custom renderers will continue to function with the legacy renderers.
1. Specifying a different `View.Visual`, such as `Material`, that uses different renderers. For more information about Material Visual, see [Xamarin.Forms Material Visual](~/xamarin-forms/user-interface/visual/material-visual.md).

## Related links

- [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
