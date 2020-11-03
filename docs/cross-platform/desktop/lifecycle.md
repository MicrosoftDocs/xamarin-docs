---
ms.assetid: 7C132A7C-4973-4B2D-98DC-3661C08EA33F
title: "WPF vs. Xamarin.Forms App Lifecycle"
description: "This document compares the similarities and differences between the application lifecycle for Xamarin.Forms and WPF applications. It also looks at the visual tree, graphics, resources, and styles."
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
---

# WPF vs. Xamarin.Forms App Lifecycle

Xamarin.Forms takes a lot of design guidance from the XAML-based frameworks that came before it, particularly WPF. However, in other ways it deviates significantly which can be a sticky point for people attempting to migrate over. This document attempts to identify some of those issues and provide guidance where possible to bridge WPF knowledge to Xamarin.Forms.

## App Lifecycle

The application lifecycle between WPF and Xamarin.Forms is similar. Both start in external (platform) code and launch the UI through a method call. The difference is that Xamarin.Forms always starts in a platform-specific assembly which then initializes and creates the UI for the app.

**WPF**

- `Main method > App > MainWindow`

> [!NOTE]
> The `Main` method is, by default, auto generated and not visible in the code.

**Xamarin.Forms**

- **iOS** &ndash; `Main method > AppDelegate > App > ContentPage`
- **Android** &ndash; `MainActivity > App > ContentPage`
- **UWP** &ndash; `Main method > App(UWP) > MainPage(UWP) > App > ContentPage`

### Application class

Both WPF and Xamarin.Forms have an `Application` class which is created as a singleton. In most cases, apps will derive from this class to provide a custom application, although this is not strictly required in WPF. Both expose an `Application.Current` property to locate the created singleton.

### Global properties + persistence

Both WPF and Xamarin.Forms have a `Application.Properties` dictionary available where you can store global app-level objects that are accessible anywhere in the application. The key difference is that Xamarin.Forms will _persist_ any primitive types stored in the collection when the app is suspended, and reload them when it is relaunched. WPF does not automatically support that behavior - instead, most developers relied on isolated storage, or utilized the built-in `Settings` support.

## Defining pages and the Visual Tree

WPF uses the `Window` as the root element for any top-level visual element. This defines an HWND in the Windows world to display information. You can create and display as many windows simultaneously as you like in WPF.

In Xamarin.Forms, the top-level visual is always defined by the platform - for example on iOS, it's a `UIWindow`. Xamarin.Forms renders it's content into these native platform representations using a `Page` class. Each `Page` in Xamarin.Forms represents a unique "page" in the application, where only one is visible at a time.

Both WPFs `Window` and Xamarin.Forms `Page` include a `Title` property to influence the displayed title, and both have an `Icon` property to display a specific icon for the page (**Note** that the title and icon are not always visible in Xamarin.Forms). In addition, you can change common visual properties on both such as the background color or image.

It is technically possible to render to two separate platform views (e.g. define two `UIWindow` objects and have the second one render to an external display or AirPlay), it requires platform-specific code to do so and is not a directly supported feature of Xamarin.Forms itself.

### Views

The visual hierarchy for both frameworks is similar. WPF is a bit deeper due to its support for WYSIWYG documents.

**WPF**

```
DependencyObject - base class for all bindable things
   Visual - rendering mechanics
      UIElement - common events + interactions
         FrameworkElement - adds layout
            Shape - 2D graphics
            Control - interactive controls
```

**Xamarin.Forms**

```
BindableObject - base class for all bindable things
   Element - basic parent/child support + resources + effects
      VisualElement - adds visual rendering properties (color, fonts, transforms, etc.)
         View - layout + gesture support
```

### View Lifecycle

Xamarin.Forms is primarily oriented around mobile scenarios. As such, applications are _activated_, _suspended_, and _reactivated_ as the user interacts with them. This is similar to clicking away from the `Window` in a WPF application and there are a set of methods and corresponding events you can override or hook into to monitor this behavior.

| Purpose | WPF Method | Xamarin.Forms Method |
|--- |--- |--- |
|Initial activation|ctor + Window.OnLoaded|ctor + Page.OnStart|
|Shown|Window.IsVisibleChanged|Page.Appearing|
|Hidden|Window.IsVisibleChanged|Page.Disappearing|
|Suspend/Lost focus|Window.OnDeactivated|Page.OnSleep|
|Activated/Got focus|Window.OnActivated|Page.OnResume|
|Closed|Window.OnClosing + Window.OnClosed|n/a|

Both support hiding/showing child controls as well, in WPF it's a tri-state property `IsVisible` (visible, hidden, and collapsed). In Xamarin.Forms, it's just visible or hidden through the `IsVisible` property.

### Layout

Page layout occurs in the same 2-pass (Measure/Arrange) that happens in WPF. You can hook into the page layout by overriding the following methods in the Xamarin.Forms `Page` class:

| Method | Purpose |
|--- |--- |
|OnChildMeasureInvalidated|Preferred size of a child has changed.|
|OnSizeAllocated|Page has been assigned a width/height.|
|LayoutChanged event|Layout/size of the Page has changed.|

There is no global layout event which is called today, nor is there a global `CompositionTarget.Rendering` event like found in WPF.

#### Common layout properties

WPF and Xamarin.Forms both support `Margin` to control spacing around an element, and `Padding` to control spacing _inside_ an element. In addition, most of the Xamarin.Forms layout views have properties to control spacing (e.g. row or column).

In addition, most elements have properties to influence how they are positioned in the parent container:

| WPF | Xamarin.Forms | Purpose |
|--- |--- |--- |
|HorizontalAlignment|HorizontalOptions|Left/Center/Right/Stretch options|
|VerticalAlignment|VerticalOptions|Top/Center/Bottom/Stretch options|

> [!NOTE]
> The actual interpretation of these properties depends on the parent container.

#### Layout views

WPF and Xamarin.Forms both use layout controls to position child elements. In most cases, these are very close to each other in terms of functionality.

| WPF | Xamarin.Forms | Layout Style |
|--- |--- |--- |
|StackPanel|StackLayout|Left-to-right, or top-to-bottom infinite stacking|
|Grid|Grid|Tabular format (rows and columns)|
|DockPanel|n/a|Dock to edges of window|
|Canvas|AbsoluteLayout|Pixel/Coordinate positioning|
|WrapPanel|n/a|Wrapping stack|
|n/a|RelativeLayout|Relative rule-based positioning|

> [!NOTE]
> Xamarin.Forms does not support a `GridSplitter`.

Both platforms use _attached properties_ to fine-tune children.

### Rendering

The rendering mechanics for WPF and Xamarin.Forms are radically different. In WPF, the controls you create directly render content to pixels on the screen. WPF maintains two object graphs (_trees_) to represent this - the _logical tree_ represents the controls as defined in code or XAML, and the _visual tree_ represents the actual rendering that occurs on the screen which is performed either directly by the visual element (through a virtual draw method), or through a XAML-defined `ControlTemplate` which can be replaced or customized. Typically, the visual tree is more complex as it includes things such as borders around controls, labels for implicit content, etc. WPF includes a set of APIs (`LogicalTreeHelper` and `VisualTreeHelper`) to examine these two object graphs.

In Xamarin.Forms, the controls you define in a `Page` are really just simple data objects. They are similar to the logical tree representation, but never render content on their own. Instead, they are the _data model_ which influences the rendering of elements. The actual rendering is done by a [separate set of _visual renderers_ which are mapped to each control type](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). These renderers are registered in each of the platform-specific projects by platform-specific Xamarin.Forms assemblies. You can see a list [here](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md). In addition to replacing or extending the renderer, Xamarin.Forms also has support for [Effects](~/xamarin-forms/app-fundamentals/effects/index.md) which can be used to influence the native rendering on a per-platform basis.

#### The Logical/Visual Tree

There is no exposed API to walk the logical tree in Xamarin.Forms - but you can use Reflection to get the same information. For example, [here's a method which can enumerate logical children](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Extensions/ElementExtensions.cs#L108) with reflection.

## Graphics

Xamarin.Forms includes a graphics system for drawing primitives, that's called Shapes. For more information about Shapes, see [Xamarin.Forms Shapes](~/xamarin-forms/user-interface/shapes/index.md). In addition, you can include 3rd party libraries such as [SkiaSharp](~/xamarin-forms/user-interface/graphics/index.md) to get cross-platform 2D drawing.

## Resources

WPF and Xamarin.Forms both have the concept of resources and resource dictionaries. You can place any object type into a `ResourceDictionary` with a key and then look it up with `{StaticResource}` for things which will not change, or `{DynamicResource}` for things which can change in the dictionary at runtime. The usage and mechanics are the same with one difference: Xamarin.Forms requires that you define the `ResourceDictionary` to assign to the `Resources` property whereas WPF pre-creates one and assigns it for you.

For example, see the definition below:

**WPF**

```xaml
<Window.Resources>
   <Color x:Key="redColor">#ff0000</Color>
   ...
</Window.Resources>
```

**Xamarin.Forms**

```xaml
<ContentPage.Resources>
   <ResourceDictionary>
      <Color x:Key="redColor">#ff0000</Color>
      ...
   </ResourceDictionary>
</ContentPage.Resources>
```

If you do not define the `ResourceDictionary`, a runtime error is generated.

## Styles

Styles are also fully supported in Xamarin.Forms and can be used to theme the Xamarin.Forms elements that make up the UI. They support triggers (property, event and data), inheritance through `BasedOn`, and resource lookups for values. Styles are applied to elements either explicitly through the `Style` property, or implicitly by not supplying a resource key - just like WPF.

### Device Styles

WPF has a set of predefined properties (stored as static values on a set of static classes such as `SystemColors`) which dictate system colors, fonts and metrics in the form of values and resource keys. Xamarin.Forms is similar, but defines a set of [Device Styles](~/xamarin-forms/user-interface/styles/device.md) to represent the same things. These styles are supplied by the framework and set to values based on the runtime environment (e.g. accessibility).

**WPF**

```xaml
<Label Text="Title" Foreground="{DynamicResource {x:Static SystemColors.DesktopBrushKey}}" />
```

**Xamarin.Forms**

```xaml
<Label Text="Title" Style="{DynamicResource TitleStyle}" />
```
