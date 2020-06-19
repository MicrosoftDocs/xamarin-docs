---
title: "Xamarin.Forms Shapes: Path"
description: "The Xamarin.Forms Path class can be used to draw curves and complex shapes."
ms.prod: xamarin
ms.assetid: B29486F4-9A5E-4588-ABDF-7EB1E69B9AE6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shapes: Path

![](~/media/shared/preview.png "This API is currently pre-release")

[![Download Sample](~/media/shared/download.png) Download the sample](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

The `Path` class derives from the `Shape` class, and can be used to draw curves and complex shapes. These curves and shapes are often described using `Geometry` objects. For information on the properties that the `Path` class inherits from the `Shape` class, see [Xamarin.Forms Shapes](index.md).

`Path` defines the following properties:

- `Data`, of type `Geometry`, which specifies the shape to be drawn.
- `RenderTransform`, of type `Transform`, which represents the transform that is applied to the geometry of a path prior to it being drawn.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

For more information about transforms, see [Xamarin.Forms Path Transforms](path-transforms.md).

## Create a Path

The following XAML example shows how to draw a polygon using a special abbreviated syntax known as path markup syntax:

```xaml
<Path Data="M 10,50 L 200,70"
      Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100" />
```

The `Data` string begins with the "moveto" command, indicated by `M`, which establishes a start point for the path. `L` is the line command, which creates a straight line from the start point to the specified end point.

> [!NOTE]
> Path markup syntax is only available in XAML.

For more information about path markup syntax, see [Xamarin.Forms Path markup syntax](path-markup-syntax.md).

## Path geometry

Curves and shapes can be described using `Geometry` objects, which are used to set the `Path` object's `Data` property.

There are a variety of `Geometry` objects to choose from. The `EllipseGeometry`, `LineGeometry`, and `RectangleGeometry` classes describe relatively simple shapes. To create more complex shapes or create curves, use a `PathGeometry`.

`PathGeometry` objects are comprised of one or more `PathFigure` objects. Each `PathFigure` object represents a different shape. Each `PathFigure` object is itself comprised of one or more `PathSegment` objects, each representing a connection portion of the shape. Segment types include the following: `LineSegment`, `BezierSegment`, and `ArcSegment`.

In the following example, a `Path` is used to draw a triangle:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure IsClosed="True"
                                StartPoint="10,100">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="100,100" />
                                <LineSegment Point="100,50" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

For more information about geometries, see [Xamarin.Forms Path geometries](path-geometries.md).

## Related links

- [ShapeDemos (sample)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.Forms Shapes](index.md)
- [Xamarin.Forms Path geometries](path-geometries.md)
- [Xamarin.Forms Path markup syntax](path-markup-syntax.md)
- [Xamarin.Forms Path transforms](path-transforms.md)
