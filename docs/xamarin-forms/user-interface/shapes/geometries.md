---
title: "Xamarin.Forms Shapes: Geometries"
description: "Xamarin.Forms geometry classes enable you to describe the geometry of a 2D shape."
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shapes: Geometries

![](~/media/shared/preview.png "This API is currently pre-release")

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

The `Geometry` class, and the classes that derive from it, enable you to describe the geometry of a 2D shape. `Geometry` objects can be simple, such as rectangles and circles, or composite, created from two or more geometry objects. More complex geometries can be created by using the `PathGeometry` class, which enables you to describe arcs and curves.

The `Geometry` and `Shape` classes seem similar, in that they both describe 2D shapes, but have an important difference. The `Geometry` class derives from the `BindableObject` class, while the `Shape` class derives from the `View` class. Therefore, `Shape` objects can render themselves and participate in the layout system, while `Geometry` objects cannot. While `Shape` objects are more readily usable than `Geometry` objects, `Geometry` objects are more versatile. While a `Shape` object is used to render 2D graphics, a `Geometry` object can be used to define the geometric region for 2D graphics, and define a region for clipping.

The base class for all geometries is the abstract class `Geometry`. The classes that derive from this class can be grouped into three categories: simple geometries, path geometries, and composite geometries.

## Simple geometries

The simple geometry classes are `EllipseGeometry`, `LineGeometry`, and `RectangleGeometry`. They are used to create basic geometric shapes, such as circles, lines, and rectangles. These same shapes, as well as more complex shapes, can be created using a `PathGeometry` or by combining geometry objects together, but these classes provide a simpler means for producing these basic geometric shapes.

### EllipseGeometry

An ellipse geometry represents the geometry or an ellipse or circle, and is defined by a center point, an x-radius, and a y-radius.

The `EllipseGeometry` class defines the following properties:

- `Center`, of type `Point`, which represents the center point of the geometry.
- `RadiusX`, of type `double`, which represents the x-radius value of the geometry. The default value of this property is 0.0.
- `RadiusY`, of type `double`, which represents the y-radius value of the geometry. The default value of this property is 0.0.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

The following example shows how to create and render an `EllipseGeometry`:

```xaml
<Path Fill="Blue"
      Stroke="Red"
      StrokeThickness="1">
  <Path.Data>
    <EllipseGeometry Center="50,50"
                     RadiusX="50"
                     RadiusY="50" />
  </Path.Data>
</Path>
```

In this example, the center of the `EllipseGeometry` is set to (50,50) and the x-radius and y-radius are both set to 50. This creates a circle with a diameter of 100, whose interior is painted blue.

### LineGeometry

A line geometry represents the geometry of a line, and is defined by specifying the start point of the line and the end point.

The `LimeGeometry` class defines the following properties:

- `StartPoint`, of type `Point`, which represents the start point of the line.
- `EndPoint`, of type `Point`, which represents the end point of the line.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

The following example shows how to create and render a `LineGeometry`:

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <LineGeometry StartPoint="10,20"
                  EndPoint="100,130" />
  </Path.Data>
</Path>
```

In this example, a `LineGeometry` is drawn from (10,20) to (100,130).

### RectangleGeometry

A rectangle geometry represents a rectangle, and is defined with a `Rect` structure that specifies its relative position and its height and width.

The `RectangleGeometry` class defines the following properties:

- `Rect`, of type `FormsRect`, which represents the dimensions of the rectangle.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

The following example shows how to create and render a `RectangleGeometry`:

```xaml
<Path Fill="Blue"
      Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <RectangleGeometry Rect="50,50,25,25" />
  </Path.Data>
</Path>
```

The position and dimensions of the rectangle are defined by a `Rect` structure. In this example, the position is (50,50), and the height and width are both 25, which creates a square.

## Path geometries

A path geometry describes a complex shape that can be composed of arcs, curves, ellipses, lines, and rectangles.

The `PathGeometry` class defines the following properties:

- `Figures`, of type `PathFigureCollection`, which represents the collection of `PathFigure` objects that describe the path's contents.
- `FillRule`, of type `FillRule`, which determines how the intersecting areas contained in the geometry are combined. The default value of this property is `FillRule.EvenOdd`.

> [!NOTE]
> The `Figures` property is the `ContentProperty` of the `PathGeometry` class, and so does not need to be explicitly set from XAML.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

A `PathGeometry` is made up of a collection of `PathFigure` objects, with each `PathFigure` describing a shape in the geometry. Each `PathFigure` is itself comprised of one or more `PathSegment` objects, each of which describes a segment of the shape. There are many types of segments:

- `ArcSegment`, which creates an elliptical arc between two points.
- `BezierSegment`, which creates a cubic Bezier curve between two points.
- `LineSegment`, which creates a line between two points.
- `PolyBezierSegment`, which creates a series of cubic Bezier curves.
- `PolyLineSegment`, which creates a series of lines.
- `PolyQuadraticBezierSegment`, which creates a series of quadratic Bezier curves.
- `QuadraticBezierSegment`, which creates a quadratic Bezier curve.

The segments within a `PathFigure` are combined into a single geometric shape with the end point of each segment being the start point of the next segment. The `StartPoint` property of a `PathFigure` specifies the point from which the first segment is drawn. Each subsequent segment starts at the end point of the previous segment. For example, a vertical line from `10,50` to `10,150` can be defined by setting the `StartPoint` property to `10,50` and creating a `LineSegment` with a `Point` property setting of `10,150`:

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,50">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="10,150" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

More complex geometries can be created by using a combination of `PathSegment` objects, and by using multiple `PathFigure` objects within a `PathGeometry`.

## Composite geometries

Composite geometry objects can be created using a `GeometryGroup`. The `GeometryGroup` class creates a composite geometry from one or more `Geometry` objects. Any number of `Geometry` objects can be added to a `GeometryGroup`.

The following example shows how to combine geometries in a `GeometryGroup`:

```xaml
<Path Stroke="Green"
      StrokeThickness="2"
      Fill="Orange">
    <Path.Data>
        <GeometryGroup>
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,250" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,250" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

In this example, four `EllipseGeometry` objects with identical x-radius and y-radius coordinates, but with different center coordinates, are combined. This creates four overlapping circles, whose interiors are filled orange with the `EvenOdd` fill rule.

## Other features

The `GeometryHelper` class provides the following helper methods:

- `FlattenGeometry`
- `FlattenCubicBezier`
- `FlattenQuadraticBezier`
- `FlattenArc`

## Related links

- [ShapeDemos (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.Forms Shapes](index.md)
