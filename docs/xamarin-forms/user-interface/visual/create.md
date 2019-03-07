---
title: "Create a Xamarin.Forms Visual Renderer"
description: "Xamarin.Forms Visual enables renderers to be selectively applied to VisualElement objects, without having to subclass Xamarin.Forms controls."
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2019
---

# Xamarin.Forms Visual

Xamarin.Forms Visual enables renderers to be selectively applied to `VisualElement` objects, without having to subclass Xamarin.Forms controls. Any renderers that specify a `VisualType` as part of the `ExportRendererAttribute` will be used to renderer views, rather than the default renderers. At renderer selection time, the `Visual` property of the view is inspected and included in the renderer selection process.

> [!IMPORTANT]
> Currently the `Visual` cannot be changed after the control has been rendered, but this will change in a future release.

Xamarin.Forms includes a visual appearance based on material design. For more information, see [Xamarin.Forms Material Visual](material-visual.md).

## Create a Visual

In your cross platform library, create a type that derives from `IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

## Register the Visual type

Create a renderer for the required view, and register the Visual type as part of the platforms `ExportRenderAttribute`:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ContentPage), typeof(CustomPageRenderer), new[] { typeof(CustomVisual) })]
namespace MyApp.Android
{
    public class CustomPageRenderer : PageRenderer
    {
        ...
    }
}
```

## Consume the Visual

Applications can opt into using the custom Visual through the `Visual` property:

```xaml
<ContentPage ...
             Visual="Custom">
    ...
</ContentPage>
```

The equivalent C# code is:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = new CustomVisual();
```

## Register a name for a Visual

There may be conflicts between different Visual libraries or perhaps you just want to refer to a Visual by a different name. In this scenario, you can use an assembly attribute to register a different name for a given Visual:

```csharp
[assembly: Visual("MyMaterialName", typeof(VisualMarker.MaterialVisual))]
```

The custom Visual can then be consumed through its registered name:

```xaml
<ContentPage ...
             Visual="MyMaterialName">
    ...
</ContentPage>
````

## Related links

- [Xamarin.Forms Material Visual](material-visual.md)
- [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
