---
title: "Xamarin.Forms Shapes: Path Transforms"
description: "A Xamarin.Forms transform defines how to transform a Path object from one coordinate space to another coordinate space."
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shapes: Path Transforms

![](~/media/shared/preview.png "This API is currently pre-release")

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

A `Transform` defines how to transform a `Path` object from one coordinate space to another coordinate space. This mapping is described by a transformation `Matrix`, which is a collection of three rows with three columns of `double` values.

A 3x3 matrix is used for transformations in a 2D x-y plane. Affine transformation matrices can be multiplied to form any number of linear transformations, such as rotation and skew, followed by translation. The following table shows the structure of a Xamarin.Forms matrix:

| | | |
|---------|---------|-----|
| M11     | M12     | 0.0 |
| M21     | M22     | 0.0 |
| OffsetX | OffsetY | 1.0 |

By manipulating matrix values, you can rotate, scale, skew, and translate `Path` objects. For example, if you change the `OffsetX` value to 100, you can use it move a `Path` object 100 device-independent units along the x-axis. If you change the `M22` value to 3, you can use it to stretch a `Path` object to three times its current height. If you change both values, you move the `Path` object 100 device-independent units along the x-axis and stretch its height by a factor of 3.

> [!NOTE]
> An affine transformation matrix has its final column equal to (0,0,1), so only the members in the first two columns need to be specified. The members in the final row, `OffsetX` and `OffsetY`, represent translation values.

Although you can use a `Matrix` structure directly to translate individual points, Xamarin.Forms also provides the following classes that enable you to transform `Path` objects without working directly with matrices:

- `RotateTransform`, which rotates a `Path` by a specified `Angle`.
- `ScaleTransform`, which scales a `Path` object by specified `ScaleX` and `ScaleY` amounts.
- `SkewTransform`, which skews a `Path` object by specified `AngleX` and `AngleY` amounts.
- `TranslateTransform`, which moves a `Path` object by specified `X` and `Y` amounts.

Xamarin.Forms also provides the following classes for creating more complex transformations:

- `TransformGroup`, which represents a composite `Transform` composed of other `Transform` objects.
- `CompositeTransform`, which represents a composite `Transform` composed of other `Transform` objects.
- `MatrixTransform`, which creates custom transforms that are not provided by the other `Transform` classes.

All of these classes derive from the `Transform` class, which defines a `Value` property of type `Matrix`. This property represents the current transformation as a `Matrix` object.

To apply a transform to a `Path`, you create a transform class and set it as the value of the `Path.RenderTransform` property.

## Rotation transform

A rotate transform rotates a `Path` object clockwise about a specified point in a 2D x-y coordinate system.

The `RotateTransform` class, which derives from the `Transform` class, defines the following properties:

- `Angle`, of type `double`, represents the angle, in degrees, of clockwise rotation. The default value of this property is 0.0.
- `CenterX`, of type `double`, represents the x-coordinate of the rotation center point. The default value of this property is 0.0.
- `CenterY`, of type `double`, represents the y-coordinate of the rotation center point. The default value of this property is 0.0.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

The `CenterX` and `CenterY` properties specify the point about which the `Path` object is rotated. This center point is expressed in the coordinate space of the object that's transformed. By default, the rotation is applied to (0,0), which is the upper-left corner of the `Path` object.

The following example shows how to rotate a `Path` object:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <RotateTransform CenterX="0"
                         CenterY="0"
                         Angle="45" />
    </Path.RenderTransform>
</Path>
```

In this example, the `Path` object is rotated 45 degrees about its upper-left corner.

## Scale transform

A scale transform scales a `Path` object in the 2D x-y coordinate system.

The `ScaleTransform` class, which derives from the `Transform` class, defines the following properties:

- `ScaleX`, of type `double`, which represents the x-axis scale factor. The default value of this property is 1.0.
- `ScaleY`, of type `double`, which represents the y-axis scale factor. The default value of this property is 1.0.
- `CenterX`, of type `double`, which represents the x-coordinate of the center point of this transform. The default value of this property is 0.0.
- `CenterY`, of type `double`, which represents the y-coordinate of the center point of this transform. The default value of this property is 0.0.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

The value of `ScaleX` and `ScaleY` have a huge impact on the resulting scaling:

- Values between 0 and 1 decrease the width and height of the scaled object.
- Values greater than 1 increase the width and height of the scaled object.
- Values of 1 indicate that the object is not scaled.
- Negative values flip the scale object horizontally and vertically.
- Values between 0 and -1 flip the scale object and decrease its width and height.
- Values less than -1 flip the object and increase its width and height.
- Values of -1 flip the scaled object but do not change its horizontal or vertical size.

The `CenterX` and `CenterY` properties specify the point about which the `Path` object is scaled. This center point is expressed in the coordinate space of the object that's transformed. By default, scaling is applied to (0,0), which is the upper-left corner of the `Path` object. This has the effect of moving the `Path` object and making it appear larger, because when you apply a transform you change the coordinate space in which the `Path` object resides.

The following example shows how to scale a `Path` object:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <ScaleTransform CenterX="0"
                        CenterY="0"
                        ScaleX="2"
                        ScaleY="2" />
    </Path.RenderTransform>
</Path>
```

In this example, the `Path` object is scaled to double the size.

## Skew transform

A skew transform skews a `Path` object in the 2D x-y coordinate system, and is useful for creating the illusion of 3D depth in a 2D object.

The `SkewTransform` class, which derives from the `Transform` class, defines the following properties:

- `AngleX`, of type `double`, which represents the x-axis skew angle, which is measured in degrees counterclockwise from the y-axis. The default value of this property is 0.0.
- `AngleY`, of type `double`, which represents the y-axis skew angle, which is measured in degrees counterclockwise from the x-axis. The default value of this property is 0.0.
- `CenterX`, of type `double`, which represents the x-coordinate of the transform center. The default value of this property is 0.0.
- `CenterY`, of type `double`, which represents the y-coordinate of the transform center. The default value of this property is 0.0.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

To predict the effect of a skew transformation, consider that `AngleX` skews x-axis values relative to the original coordinate system. Therefore, for an `AngleX` of 30, the y-axis rotates 30 degrees through the origin and skews the values in x by 30 degrees from that origin. Similarly, an `AngleY` of 30 skews the y values of the `Path` object by 30 degrees from the origin.

To skew a `Path` object in place, set the `CenterX` and `CenterY` properties to the object's center point.

The following example shows how to skew a `Path` object:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <SkewTransform CenterX="0"
                       CenterY="0"
                       AngleX="45"
                       AngleY="0" />
    </Path.RenderTransform>
</Path>
```

In this example, a horizontal skew of 45 degrees is applied to the `Path` object, from a center point of (0,0).

## Translate transform

A translate transform moves an object in the 2D x-y coordinate system.

The `TranslateTransform` class, which derives from the `Transform` class, defines the following properties:

- `X`, of type `double`, which represents the distance to move along the x-axis. The default value of this property is 0.0.
- `Y`, of type `double`, which represents the distance to move along the y-axis. The default value of this property is 0.0.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

Negative `X` values move an object to the left, while positive values move an object to the right. Negative `Y` values move an object up, while positive values move an object down.

The following example shows how to translate a `Path` object:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <TranslateTransform X="50"
                            Y="50" />
    </Path.RenderTransform>
</Path>
```

In this example, the `Path` object is moved 50 device-independent units to the right, and 50 device-independent units down.

## Apply multiple transforms

Xamarin.Forms has two classes that support applying multiple transforms to a `Path` object. These are `TransformGroup`, and `CompositeTransform`. A `TransformGroup` performs transforms in any desired order, while a `CompositeTransform` performs transforms in a specific order.

### Transform groups

Transform groups represent composite transforms composed of multiple `Transform` objects.

The `TransformGroup` class, which derives from the `Transform` class, defines the following properties:

- `Children`, of type `TransformCollection`, which represents a collection of `Transform` objects.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

The order of transformations is important in a composite transform that uses the `TransformGroup` class. For example, if you first rotate, then scale, then translate, you get a different result than if you first translate, then rotate, then scale. One reason order is significant is that transforms like rotation and scaling are performed respect to the origin of the coordinate system. Scaling an object that is centered at the origin produces a different result to scaling an object that has been moved away from the origin. Similarly, rotating an object that is centered at the origin produces a different result than rotating an object that has been moved away from the origin.

The following example shows how to perform a composite transform using the `TransformGroup` class:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <TransformGroup>
            <ScaleTransform ScaleY="2" />
            <RotateTransform Angle="45" />
        </TransformGroup>
    </Path.RenderTransform>
</Path>
```

In this example, the `Path` object is scaled to twice its size, and rotated by 45 degrees.

## Composite transforms

A composite transform applies multiple transforms to an object.

The `CompositeTransform` class, which derives from the `Transform` class, defines the following properties:

- `CenterX`, of type `double`, which represents the x-coordinate of the center point of this transform. The default value of this property is 0.0.
- `CenterY`, of type `double`, which represents the y-coordinate of the center point of this transform. The default value of this property is 0.0.
- `ScaleX`, of type `double`, which represents the x-axis scale factor. The default value of this property is 1.0.
- `ScaleY`, of type `double`, which represents the y-axis scale factor. The default value of this property is 1.0.
- `SkewX`, of type `double`, which represents the x-axis skew angle, which is measured in degrees counterclockwise from the y-axis. The default value of this property is 0.0.
- `SkewY`, of type `double`, which represents the y-axis skew angle, which is measured in degrees counterclockwise from the x-axis. The default value of this property is 0.0.
- `Rotation`, of type `double`, represents the angle, in degrees, of clockwise rotation. The default value of this property is 0.0.
- `TranslateX`, of type `double`, which represents the distance to move along the x-axis. The default value of this property is 0.0.
- `TranslateY`, of type `double`, which represents the distance to move along the y-axis. The default value of this property is 0.0.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

A `CompositeTransform` applies transforms in this order:

1. Scale (`ScaleX` and `ScaleY`).
1. Skew (`SkewX` and `SkewY`).
1. Rotate (`Rotation`).
1. Translate (`TranslateX`, `TranslateY`).

If you want to apply multiple transforms to an object in a different order, you should create a `TransformGroup` and insert the transforms in your intended order.

> [!IMPORTANT]
> A `CompositeTransform` uses the same center points, `CenterX` and `CenterY`, for all transformations. If you want to specify different center points per transform, use a `TransformGroup`,

The following example shows how to perform a composite transform using the `CompositeTransform` class:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <CompositeTransform ScaleX="2"
                            ScaleY="2"
                            Rotation="45"
                            TranslateX="50"
                            TranslateY="50" />
    </Path.RenderTransform>
</Path>
```

In this example, the `Path` object is scaled to twice its size, rotated by 45 degrees, and translated by 50 device-independent units.

## Custom transforms

A matrix transform manipulates objects or coordinate systems in a 2D plane, using an affine matrix. A 3x3 matrix is used for transformations. You can multiply affine matrix transformations to form linear transformations, such as rotation and skew that are followed by translation.

The `MatrixTransform` class, which derives from the `Transform` class, defines the following properties:

- `Matrix`, of type `Matrix`, which represents the matrix that defines the transformation.

This properties is backed by a [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) object, which means that it can be targets of data bindings, and styled.

The `MatrixTransform` class is used to create custom transformation that aren't provided by the `RotateTransform`, `ScaleTransform`, `SkewTransform`, or `TranslateTransform` classes.

The following example shows how to transform a `Path` object using a `MatrixTransform`:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform>
            <MatrixTransform.Matrix>
                <!-- M11 stretches, M12 skews -->
                <Matrix OffsetX="10"
                        OffsetY="100"
                        M11="3"
                        M12="2" />
            </MatrixTransform.Matrix>
        </MatrixTransform>
    </Path.RenderTransform>
</Path>
```

In this example, the `Path` object is stretched, skewed, and offset in both the X and Y dimensions.

Alternatively, this can be written in a simplified form that uses a type converter which is built into Xamarin.Forms:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Center"
      HeightRequest="100"
      WidthRequest="100"
      Data="M13.908992,16.207977L32.000049,16.207977 32.000049,31.999985 13.908992,30.109983z">
    <Path.RenderTransform>
        <MatrixTransform Matrix="3,2,0,1,10,100" />
    </Path.RenderTransform>
</Path>
```

In this example, the `Matrix` property is specified as a comma-delimited string consisting of six members: `M11`, `M12`, `M21`, `M22`, `OffsetX`, `OffsetY`.

## Related links

- [ShapeDemos (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
- [Xamarin.Forms Shapes](index.md)
