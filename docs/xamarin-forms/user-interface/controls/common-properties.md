---
title: "Xamarin.Forms common control properties, methods, and events"
description: "This article describes common properties, methods, and events defined on the VisualElement class, that are commonly used in deriving classes."
ms.prod: xamarin
ms.assetId: 85A0CCF5-C1D8-40BB-927F-A4D944E5534D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/22/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms common control properties, methods, and events

The Xamarin.Forms `VisualElement` class is the base class for most of the controls used in a Xamarin.Forms application. The `VisualElement` class defines many [properties](#properties), [methods](#methods), and [events](#events) that are used in deriving classes.

## Properties

The following properties are available on [`VisualElement`](xref:Xamarin.Forms.VisualElement) objects.

### [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)

The `AnchorX` property is a `double` value that defines the center point on the X axis for transforms such as scale and rotation. The default value is 0.5.

### [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

The `AnchorY` property is a `double` value that defines the center point on the Y axis for transforms such as scale and rotation. The default value is 0.5.

### `Background`

The `Background` property is a `Brush` value that enables brushes to be used as the background in any control. The default value is `Brush.Default`.

### [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)

The `BackgroundColor` property is a `Color` that determines the background color of the control. If unset, the background will be the default `Color` object, which renders as transparent.

### [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)

The `Behaviors` property is a `List` of `Behavior` objects. Behaviors enable you to attach reusable functionality to elements by adding them to the `Behaviors` list. For more information about the `Behavior` class, see [Xamarin.Forms Behaviors](~/xamarin-forms/app-fundamentals/behaviors/index.md).

### [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds)

The `Bounds` property is a read-only `Rectangle` object that represents the space occupied by the control. The `Bounds` property value is assigned during the layout cycle. The `Rectangle` `struct` contains useful properties and methods for testing intersection and containment of rectangles. For more information, see the [Xamarin.Forms Rectangle API](xref:Xamarin.Forms.Rectangle).

### `Clip`

The `Clip` property is a `Geometry` object that defines the outline of the contents of an element. To define a clip, use a `Geometry` object such as `EllipseGeometry` to set the element's `Clip` property. Only the area that is within the region of the geometry will be visible. For more information, see [Clip with a Geometry](~/xamarin-forms/user-interface/shapes/geometries.md#clip-with-a-geometry).

### [`Effects`](xref:Xamarin.Forms.Element.Effects)

The `Effects` property is a `List` of `Effect` objects, inherited from the [`Element`](xref:Xamarin.Forms.Element) class. Effects allow native controls to be customized, and are typically used for small styling changes. For more information about the `Effect` class, see [Xamarin.Forms Effects](~/xamarin-forms/app-fundamentals/effects/index.md).

### [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)

The `FlowDirection` property is a `FlowDirection` enum value. Flow direction can be set to `MatchParent`, `LeftToRight`, or `RightToLeft` and determines the layout order and direction. The `FlowDirection` property is typically used to support languages that read right-to-left.

### [`Height`](xref:Xamarin.Forms.VisualElement.Height)

The `Height` property is a read-only `double` value that describes the rendered height of the control. The `Height` property is calculated during the layout cycle and can't be directly set. The height of a control can be requested using the [HeightRequest property](#heightrequest).

### [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)

The `HeightRequest` property is a `double` value that determines the desired height of the control. The absolute height of the control may not match the requested value. For more information, see [Request properties](#request-properties).

### [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent)

The `InputTransparent` property is a `bool` that determines whether the control receives user input. The default value is `false`, ensuring that the element receives input. This property transfers to child elements when it's set. Setting the `InputTransparent` property to `true` on a layout class will result in all elements within the layout not receiving input.

### [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)

The `IsEnabled` property is a `bool` value that determines whether the control reacts to user input. The default value is `true`. Setting this property to false will prevent the control from accepting user input.

### [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)

The `IsFocused` property is a `bool` value that describes whether the control is currently the focused object. Calling the [`Focus`](#focus) method on the control will result in the `IsFocused` value being set to true. Calling the [`Unfocus`](#unfocus) method will set this property to false.

### [`IsTabStop`](xref:Xamarin.Forms.VisualElement.IsTabStop)

The `IsTabStop` property is a `bool` value that defines whether the control receives focus when the user is advancing through controls with the tab key. If this property is false, the `TabIndex` property will have no effect.

### [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible)

The `IsVisible` property is a `bool` value that determines whether the control is rendered. Controls with the `IsVisible` property set to false won't be displayed, won't be considered for space calculations during the layout cycle, and can't accept user input.

### [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest)

The `MinimumHeightRequest` property is a `double` value that determines how overflow is handled when two elements are competing for limited space. Setting the `MinimumHeightRequest` property allows the layout process to scale the element down to the minimum dimension requested. If no `MinimumHeightRequest` is specified, the default value is -1 and the layout process will consider the `HeightRequest` to be the minimum value. This means that elements with no `MinimumHeightRequest` value will not have scalable height.

For more information, see [Minimum request properties](#minimum-request-properties).

### [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest)

The `MinimumWidthRequest` property is a `double` value that determines how overflow is handled when two elements are competing for limited space. Setting the `MinimumWidthRequest` property allows the layout process to scale the element down to the minimum dimension requested. If no `MinimumWidthRequest` is specified, the default value is -1 and the layout process will consider the `WidthRequest` to be the minimum value. This means that elements with no `MinimumWidthRequest` value will not have scalable width.

For more information, see [Minimum request properties](#minimum-request-properties).

### [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)

The `Opacity` property is a `double` value from zero to one that determines the opacity of the control during rendering. The default value for this property is 1.0. Values outside of the range from 0 to 1 will be clamped. The `Opacity` property is only applied if the [`IsVisible`](#isvisible) property is `true`. Opacity is applied iteratively. Therefore if a parent control has 0.5 opacity and its child has 0.5 opacity, the child will render with an effective 0.25 opacity value. Setting the `Opacity` property of an input control to 0 has undefined behavior.

### [`Parent`](xref:Xamarin.Forms.Element.Parent)

The `Parent` property is inherited from the `Element` class. This property is an `Element` object that is the parent of control. The `Parent` property is typically set automatically on an element when it's added as a child of another element.

### [`Resources`](xref:Xamarin.Forms.VisualElement.Resources)

The `Resources` property is a `ResourceDictionary` instance that is populated with key/value pairs that are typically populated at runtime from XAML. This dictionary allows application developers to reuse objects defined in XAML at both compile time and run time. The keys in the dictionary are populated from the `x:Key` attribute of the XAML tag. The object created from XAML is inserted into the `ResourceDictionary` for the specified key. once it has been initialized.

For more information, see [Resource Dictionaries](~/xamarin-forms/xaml/resource-dictionaries.md).

### [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)

The `Rotation` property is a `double` value between zero and 360 that defines the rotation about the Z axis in degrees. The default value of this property is 0. Rotation is applied relative to the [`AnchorX`](#anchorx) and [`AnchorY`](#anchory) values.

### [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)

The `RotationX` property is a `double` value between zero and 360 that defines the rotation about the X axis in degrees. The default value of this property is 0. Rotation is applied relative to the [`AnchorX`](#anchorx) and [`AnchorY`](#anchory) values.

### [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

The `RotationY` property is a `double` value between zero and 360 that defines the rotation about the Y axis in degrees. The default value of this property is 0. Rotation is applied relative to the [`AnchorX`](#anchorx) and [`AnchorY`](#anchory) values.

### [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)

The `Scale` property is a `double` value that defines the scale of the control. The default value of this property is 1.0. Scale is applied relative to the [`AnchorX`](#anchorx) and [`AnchorY`](#anchory) values.

### [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX)

The `ScaleX` property is a `double` value that defines the scale of the control along the X axis. The default value of this property is 1.0. The `ScaleX` property is applied relative to the [`AnchorX`](#anchorx) value.

### [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY)

The `ScaleY` property is a `double` value that defines the scale of the control along the Y axis. The default value of this property is 1.0. The `ScaleY` property is applied relative to the [`AnchorY`](#anchory) value.

### [`Style`](xref:Xamarin.Forms.NavigableElement.Style)

The `Style` property is inherited from the `NavigableElement` class. This property is an instance of the `Style` class. The `Style` class contains triggers, setters, and behaviors that define the appearance and behavior of visual elements. For more information, see [Xamarin.Forms XAML Styles](~/xamarin-forms/user-interface/styles/xaml/index.md).

### [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass)

The `StyleClass` property is a list of `string` objects that represent the names of `Style` classes. This property is inherited from the `NavigableElement` class. The `StyleClass` property allows multiple style attributes to be applied to a `VisualElement` instance. For more information, see [Xamarin.Forms Style Classes](~/xamarin-forms/user-interface/styles/xaml/style-class.md).

### [`TabIndex`](xref:Xamarin.Forms.VisualElement.TabIndex)

The `TabIndex` property is an `int` value that defines the control order when advancing through controls with the tab key. The `TabIndex` property is the implementation for the property defined on the `ITabStopElement` interface, which the `VisualElement` class implements.

### [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)

The `TranslationX` property is a `double` value that defines the delta translation to be applied on the X axis. Translation is applied after layout and is typically used for applying animations. Translating an element outside the bounds of its parent container my prevent inputs from working.

For more information, see [Animation in Xamarin.Forms](~/xamarin-forms/user-interface/animation/index.md).

### [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)

The `TranslationY` property is a `double` value that defines the delta translation to be applied on the Y axis. Translation is applied after layout and is typically used for applying animations. Translating an element outside the bounds of its parent container my prevent inputs from working.

For more information, see [Animation in Xamarin.Forms](~/xamarin-forms/user-interface/animation/index.md).

### [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers)

The `Triggers` property is a read-only `List` of `TriggerBase` objects. Triggers allow application developers to express actions in XAML that change the visual appearance of controls in response to event or property changes. For more information, see [Xamarin.Forms Triggers](~/xamarin-forms/app-fundamentals/triggers.md).

### [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)

The `Visual` property is an `IVisual` instance that enables renderers to be created and selectively applied to `VisualElement` instances. The `Visual` property is set to match its parent so defining a renderer on a component will also apply to any children of that component. If no custom renderer is set on a control or its ancestors, the default Xamarin.Forms renderer will be used. For more information, see [Xamarin.Forms Visual](~/xamarin-forms/user-interface/visual/index.md).

### [`Width`](xref:Xamarin.Forms.VisualElement.Width)

The `Width` property is a read-only `double` value that describes the rendered width of the control. The `Width` property is calculated during the layout cycle and can't be directly set. The width of a control can be requested using the [WidthRequest property](#widthrequest).

### [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)

The `WidthRequest` property is a `double` value that determines the desired width of the control. The absolute width of the control may not match the requested value. For more information, see [Request properties](#request-properties).

### [`X`](xref:Xamarin.Forms.VisualElement.X)

The `X` property is a read-only `double` value that describes the current X position of the control.

### [`Y`](xref:Xamarin.Forms.VisualElement.Y)

The `Y` property is a read-only `double` value that describes the current Y position of the control.

## Methods

The following methods are available on the `VisualElement` class. For a complete list, see [VisualElement API Methods](xref:Xamarin.Forms.VisualElement#methods).

### [`FindByName`](xref:Xamarin.Forms.Element.FindByName*)

The `FindByName` method is inherited from the `Element` class and has the following signature:

```csharp
public object FindByName (string name)
```

This method searches all child elements for the provided `name` argument and returns the element that has the specified name. If no match is found, `null` is returned.

### [`Focus`](xref:Xamarin.Forms.VisualElement.Focus)

The `Focus` method attempts to set focus on the element. This method has the following signature:

```csharp
public bool Focus ()
```

The `Focus` method returns `true` if keyboard focus was successfully set and `false` if the method call did not result in a focus change. The element must be able to receive focus for this method to work. Calling the `Focus` method on elements that are offscreen or unrealized has undefined behavior.

### [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus)

The `Unfocus` method attempts to remove focus on the element. This method has the following signature:

```csharp
public void Unfocus ()
```

The element must already have focus for this method to work.

## Events

The following events are available on the `VisualElement` class. For a complete list, see [Xamarin.Forms VisualElement Events](xref:Xamarin.Forms.VisualElement#events).

### [`Focused`](xref:Xamarin.Forms.VisualElement.Focused)

The `Focused` event is raised whenever the `VisualElement` instance receives focus. This event is not bubbled through the Xamarin.Forms stack, it's received directly from the native control. This event is emitted by the [`IsFocused`](#isfocused) property setter.

### [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

The `SizeChanged` event is raised whenever the `VisualElement` instance `Height` or `Width` properties change. If developers wish to respond directly to the size change, instead of responding to the post-change event, they should implement the [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated*) virtual method instead.

### [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)

The `Unfocused` event is raised whenever the `VisualElement` instance loses focus. This event is not bubbled through the Xamarin.Forms stack, it's received directly from the native control. This event is emitted by the [`IsFocused`](#isfocused) property setter.

## Units of Measurement

Android, iOS, and UWP platforms all have different measurement units that can vary across devices. Xamarin.Forms uses a platform-independent unit of measurement that normalizes units across devices and platforms. There are 160 units per inch, or 64 units per centimeter, in Xamarin.Forms.

## Request properties

Properties whose names contain "request" define a desired value, which may not match the actual rendered value. For example, `HeightRequest` might be set to 150 but if the layout only allows room for 100 units, the rendered `Height` of the control will only be 100. Rendered size is affected by available space and contained components.

## Minimum request properties

Minimum request properties include `MinimumHeightRequest` and `MinimumWidthRequest`, and are intended to enable more precise control over how elements handle overflow relative to each other. However, layout behavior related to these properties has some important considerations.

### Unspecified minimum property values

If a minimum value is not set, the minimum property defaults to -1. The layout process ignores this value and considers the absolute value to be the minimum. The practical consequence of this behavior is that an element with no minimum value specified **will not** shrink. An element with a minimum value specified **will** shrink.

The following XAML shows two `BoxView` elements in a horizontal `StackLayout`:

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView HeightRequest="100" BackgroundColor="Purple" WidthRequest="500"></BoxView>
    <BoxView HeightRequest="100" BackgroundColor="Green" WidthRequest="500" MinimumWidthRequest="250"></BoxView>
</StackLayout>
```

The first `BoxView` instance requests a width of 500 and does not specify a minimum width. The second `BoxView` instance requests a width of 500 and a minimum width of 250. If the parent `StackLayout` element is not wide enough to contain both components at their requested width, the first `BoxView` instance will be considered by the layout process to have a minimum width of 500 because no other valid minimum is specified. The second `BoxView` instance is allowed to scale down to 250 and it will shrink to fit until its width hits 250 units.

If the desired behavior is for the first `BoxView` instance to scale down with no minimum width, the `MinimumWidthRequest` must be set to a valid value, such as 0.

### Minimum and absolute property values

The behavior is undefined when the minimum value is greater than the absolute value. For example, if `WidthRequest` is set to 100, the `MinimumWidthRequest` property should never exceed 100. When specifying a minimum property value, you should always specify an absolute value to ensure the absolute value is greater than the minimum value.

### Minimum properties within a Grid

`Grid` layouts have their own system for relative sizing of rows and columns. Using `MinimumWidthRequest` or `MinimumHeightRequest` within a `Grid` layout will not have an effect. For more information, see [Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md).

## Related links

- [VisualElement API](xref:Xamarin.Forms.VisualElement)
