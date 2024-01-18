---
title: "Create a Xamarin.Forms Visual Renderer"
description: "Create Xamarin.Forms Visuals to be selectively applied to VisualElement objects, without having to subclass Xamarin.Forms views."
ms.service: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Create a Xamarin.Forms Visual Renderer

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

Xamarin.Forms Visual enables renderers to be created and selectively applied to [`VisualElement`](xref:Xamarin.Forms.VisualElement) objects, without having to subclass Xamarin.Forms views. A renderer that specifies an `IVisual` type, as part of its `ExportRendererAttribute`, will be used to render opted in views, rather than the default renderer. At renderer selection time, the `Visual` property of the view is inspected and included in the renderer selection process.

> [!IMPORTANT]
> Currently the [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) property cannot be changed after the view has been rendered, but this will change in a future release.

The process for creating and consuming a Xamarin.Forms Visual renderer is:

1. Create platform renderers for the required view. For more information, see [Create renderers](#create-platform-renderers).
1. Create a type that derives from `IVisual`. For more information, see [Create an IVisual type](#create-an-ivisual-type).
1. Register the `IVisual` type as part of the `ExportRendererAttribute` that decorates the renderers. For more information, see [Register the IVisual type](#register-the-ivisual-type).
1. Consume the Visual renderer by setting the [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) property on the view to the `IVisual` name. For more information, see [Consume the Visual renderer](#consume-the-visual-renderer).
1. [optional] Register a name for the `IVisual` type. For more information, see [Register a name for the IVisual type](#register-a-name-for-the-ivisual-type).

## Create platform renderers

For information about creating a renderer class, see [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). However, note that a Xamarin.Forms Visual renderer is applied to a view without having to subclass the view.

The renderer classes outlined here implement a custom [`Button`](xref:Xamarin.Forms.Button) that displays its text with a shadow.

### iOS

The following code example shows the button renderer for iOS:

```csharp
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.TitleShadowOffset = new CoreGraphics.CGSize(1, 1);
            Control.SetTitleShadowColor(Color.Black.ToUIColor(), UIKit.UIControlState.Normal);
        }
    }
}
```

### Android

The following code example shows the button renderer for Android:

```csharp
public class CustomButtonRenderer : Xamarin.Forms.Platform.Android.AppCompat.ButtonRenderer
{
    public CustomButtonRenderer(Context context) : base(context)
    {
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.SetShadowLayer(5, 3, 3, Color.Black.ToAndroid());
        }
    }
}
```

## Create an IVisual type

In your cross-platform library, create a type that derives from `IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

The `CustomVisual` type can then be registered against the renderer classes, permitting [`Button`](xref:Xamarin.Forms.Button) objects to opt into using the renderers.

## Register the IVisual type

In the platform projects, add the `ExportRendererAttribute` at the assembly level:

```csharp
[assembly: ExportRenderer(typeof(Xamarin.Forms.Button), typeof(CustomButtonRenderer), new[] { typeof(CustomVisual) })]
namespace VisualDemos.iOS
{
    public class CustomButtonRenderer : ButtonRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
        {
            // ...
        }
    }
}
```

In this example for the iOS platform project, the `ExportRendererAttribute` specifies that the `CustomButtonRenderer` class will be used to render consuming [`Button`](xref:Xamarin.Forms.Button) objects, with the `IVisual` type registered as the third argument. A renderer that specifies an `IVisual` type, as part of its `ExportRendererAttribute`, will be used to render opted in views, rather than the default renderer.

## Consume the Visual renderer

A [`Button`](xref:Xamarin.Forms.Button) object can opt into using the renderer classes by setting its [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) property to `Custom`:

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> In XAML, a type converter removes the need to include the "Visual" suffix in the [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) property value. However, the full type name can also be specified.

The equivalent C# code is:

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

At renderer selection time, the [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) property of the [`Button`](xref:Xamarin.Forms.Button) is inspected and included in the renderer selection process. If a renderer isn't located, the Xamarin.Forms default renderer will be used.

The following screenshots show the rendered [`Button`](xref:Xamarin.Forms.Button), which displays its text with a shadow:

[![Screenshot of custom Button with shadow text, on iOS and Android](material-visual-images/custom-button.png "Button with shadow text")](material-visual-images/custom-button-large.png#lightbox)

## Register a name for the IVisual type

The [`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) can be used to optionally register a different name for the `IVisual` type. This approach can be used to resolve naming conflicts between different Visual libraries, or in situations where you just want to refer to a Visual by a different name than its type name.

The [`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) should be defined at the assembly level in either the cross-platform library, or in the platform project:

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

The `IVisual` type can then be consumed through its registered name:

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> When consuming a Visual through its registered name, any "Visual" suffix must be included.

## Related links

- [Material Visual (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Xamarin.Forms Material Visual](material-visual.md)
- [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)