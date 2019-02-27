---
title: "Xamarin.Forms Visual"
description: "This article introduces Xamarin.Forms Visual, which allows users to more easily define how they want controls to visuall rendere."
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2018
---

# Xamarin.Forms Visual

_This article introduces Xamarin.Forms Visual which allows users to create renderers that can be specified at runtime._

Renderers that implement the visual appearance are then used to renderer views, rather than the default renderers. At renderer selection time, the `Visual` property of the view is inspected and included in the renderer selection process. In addition, if the `Visual` property changes at runtime, the specified renderer is recreated along with any children.

> [!IMPORTANT]
> The `Visual` property is defined in the `VisualElement` class, with views inheriting the `Visual` property value from their parents. Therefore, setting the `Visual` property on a `ContentPage` ensures that any supported views in the page will use that visual appearance. In addition, the `Visual` property can be overridden on a view.

Xamarin.Forms 3.6 includes a visual appearance based on material design, with the renderers being known as the material renderers. 

### Creating your own Visual

In your cross platform library create a Visual type that the user can use to mark a Xamarin Forms Element

```C#
public class CustomVisual : IVisual
{
}

```

Now that you have created this type users can easily mark any Xamarin Forms Element

```xaml
<ContentPage ...
             Visual="Custom">
    ...
</ContentPage>
```

```c#
var contentPage = new ContentPage();
contentPage.Visual = new CustomVisual();
```

Now you just have to register the visual type against a custom renderer on the platform you want a custom renderer for


```C#
using Xamarin.Forms.Material.Android

[assembly: ExportRenderer(typeof(ContentPage), typeof(CustomPageRenderer), new[] { typeof(CustomVisual) })]
namespace MyApp.Android
{
    public class CustomPageRenderer : PageRenderer

```

### Registering your own name for a visual

At some point there may be conflicts between different Visual Libraries or maybe you just want to refer to a visual by a different name. In this case you can use an assembly attribute to register a different name for a given visual

```C#
[assembly: Visual("MyMaterialName", typeof(VisualMarker.MaterialVisual))]
```

Which will now let you do the following
```xaml
<ContentPage ...
             Visual="MyMaterialName">
    ...
</ContentPage>
```