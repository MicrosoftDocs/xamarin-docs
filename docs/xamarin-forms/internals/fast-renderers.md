---
title: "Xamarin.Forms Fast Renderers"
description: "This article introduces fast renderers, which reduce the inflation and rendering costs of a Xamarin.Forms control on Android by flattening the resulting native control hierarchy."
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
---

# Xamarin.Forms Fast Renderers

![Preview](~/media/shared/preview.png)

_This article introduces fast renderers, which reduce the inflation and rendering costs of a Xamarin.Forms control on Android by flattening the resulting native control hierarchy._

Traditionally, most of the original control renderers on Android are composed of two views:

- A native control, such as a `Button` or `TextView`.
- A container `ViewGroup` that handles some of the layout work, gesture handling, and other tasks.

However, this approach has a performance implication in that two views are created for each logical control, which results in a more complex visual tree that requires more memory, and more processing to render on screen.

Fast renderers reduce the inflation and rendering costs of a Xamarin.Forms control into a single view. Therefore, instead of creating two views and adding them to the view tree, only one is created. This improves performance by creating fewer objects, which in turn means a less complex view tree, and less memory use (which also results in fewer garbage collection pauses).

Fast renderers are available for the following controls in Xamarin.Forms 2.4 on Android:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`Frame`](xref:Xamarin.Forms.Frame)

Functionally, these fast renderers are no different to the original renderers. However, they are currently experimental and can only be used by adding the following line of code to your `MainActivity` class before calling `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> Fast renderers are only applicable to the app compat Android backend, so this setting will be ignored on pre-app compat activities.

Performance improvements will vary for each application, depending upon the complexity of the layout. For example, performance improvements of x2 are possible when scrolling through a [`ListView`](xref:Xamarin.Forms.ListView) containing thousands of rows of data, where the cells in each row are made of controls that use fast renderers, which results in visibly smoother scrolling.


## Related Links

- [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
