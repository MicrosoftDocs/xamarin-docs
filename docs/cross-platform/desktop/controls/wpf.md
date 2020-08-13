---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: "WPF vs. Xamarin.Forms: Similarities & Differences"
description: "This document compares and contrasts WPF to Xamarin.Forms. It discusses control templates, XAML, binding infrastructure, data templates, ItemsControl, UserControl, navigation, and URL navigation."
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
---

# WPF vs. Xamarin.Forms: Similarities & Differences

## Control Templates

WPF supports the concept of *Control Templates* which provide the visualization instructions for a control (`Button`, `ListBox`, etc.). As mentioned above, Xamarin.Forms uses concrete _rendering_ classes for this which interact with the native platform (iOS, Android, etc.) to visualize the control.

However, Xamarin.Forms _does_ have a `ControlTemplate` type - it is used for theming `Page` objects. It provides a definition for a `Page` that provides consistent content, but allows the user of the page to change colors, fonts, etc. and even add elements to make it unique to the application.

Common usages for this are things such as authentication dialogs, prompts and to provide a standardized, but themable page look and feel that can be customized within the app. As part of this support, many familiar WPF-named controls are used:

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

But it's important to know that these are _not_ serving the same purpose in Xamarin.Forms. For more information on this feature, check out the [documentation page](~/xamarin-forms/app-fundamentals/templates/control-template.md).

## XAML

XAML is used as the declarative markup language for WPF and Xamarin.Forms. For the most part, the syntax is identical - the primary difference is the objects that are defined/created by the XAML graphs.

- Xamarin.Forms supports the [XAML 2009 specification](/dotnet/framework/xaml-services/xaml-2009-language-features/); this makes it easier to define data such as `string`s, `int`s, etc. as well as defining generic types and passing arguments to constructors.

- There is currently no way to dynamically load XAML like WPF can with `XamlReader`. You can get the same basic functionality with a [NuGet package](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) though.

### Markup Extensions

Xamarin.Forms supports extending XAML through markup extensions, much like WPF. Out of the box, it has the same basic building blocks:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

In addition, it includes `{x:Reference}` from the XAML 2009 specification, and a `{TemplateBinding}` markup extension which is used for the specialized version of `ControlTemplate` supported by Xamarin.Forms.

> [!WARNING]
> The `ControlTemplate` support isn't the same - even though it has the same name.

Xamarin.Forms supports custom markup extensions as well - but the implementation is slightly different. In WPF, you must derive from `MarkupExtension` - an abstract base class. In Xamarin.Forms, that is replaced with an interface `IMarkupExtension` or `IMarkupExtension<T>` which is more flexible.

Just like WPF, the single required method is a `ProvideValue` method to return the value from the markup extension.

## Binding infrastructure

One of the core concepts carried over is a data binding infrastructure to connect visual properties to .NET data properties. This enables architectural patterns such as MVVM. The basic design is identical - you have a bindable base class [BindableObject](xref:Xamarin.Forms.BindableObject), in WPF this is the [DependencyObject](xref:System.Windows.DependencyObject) class. This base class is used as the root ancestor for all objects that will participate as targets in data binding. The derived classes then expose [BindableProperty](xref:Xamarin.Forms.BindableProperty) objects which act as the backing storage for property values (these are defined as [DependencyProperty](xref:System.Windows.DependencyProperty) objects in WPF).

### Defining bindable properties

The definition for a bindable property in Xamarin.Forms is the same as WPF:

1. The object must derive from `BindableObject`.
2. There must be a public static field of type `BindableProperty` declared to define the backing storage key for the property.
3. There should be a public instance property wrapper that uses `GetValue` and `SetValue` to retrieve and change the properties value.

For a complete example, see [Bindable Properties in Xamarin.Forms](~/xamarin-forms/xaml/bindable-properties.md).

### Attached properties

Attached properties are a subset of the bindable property and they work the same way they do in WPF. The primary difference is that the property wrapper is omitted in this case and replaced with a set of static get/set methods on the owning class. See [Attached Properties in Xamarin.Forms](~/xamarin-forms/xaml/attached-properties.md) for more information.

### Using the binding engine

The process for using the binding engine is the same as it is in WPF. It can be utilized in code-behind by creating a `Binding` object tied to a source object (any .NET type) and an optional property value (if omitted, it treats the source object as the property itself - just like WPF). You can then use `SetBinding` on any `BindableObject` to associate the binding to a `BindableProperty`.

Alternatively, you can define the binding relationship in XAML using the `BindingExtension`. It has the same basic values as the extension in WPF.

The binding support and engine are more similar to the Silverlight implementation than WPF. There are several missing features which were not implemented in Xamarin.Forms:

- There is no support for the following features in bindings:
  - BindingGroupName
  - BindsDirectlyToSource
  - IsAsync
  - MultiBinding
  - NotifyOnSourceUpdated
  - NotifyOnTargetUpdated
  - NotifyOnValidationError
  - UpdateSourceTrigger
  - UpdateSourceExceptionFilter
  - ValidatesOnDataErrors
  - ValidatesOnExceptions
  - ValidationRules collection
  - XPath
  - XmlNamespaceManager

#### RelativeSource

There is no support for `RelativeSource` bindings. In WPF, these allow you to bind to other visual elements defined in XAML. In Xamarin.Forms, this same capability can be achieved using the `{x:Reference}` markup extension. For example, assuming we have a control with the name "otherControl" that has a Text property, we can bind to it like this:

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

The same capability can be used for the `{RelativeSource Self}` feature. However there is no support for locating ancestors by type (`{RelativeSource FindAncestor}`).

#### Binding context

In WPF, you can define a `DataContext` property value which reprents the default binding source. If the source for a binding is not defined, this property value is used. The value is inherited down the visual tree, allowing it to be defined at a higher level and then used by children.

In Xamarin.Forms, this same feature is avaialable, but the property name is `BindingContext`.

#### Value converters

Value converters are fully supported in Xamarin.Forms - just like WPF. The same interface shape is used, but Xamarin.Forms has the interface defined in the `Xamarin.Forms` namespace.

### Model-View-ViewModel

MVVM is completely supported by both WPF and Xamarin.Forms.

WPF includes a built in `RoutedCommand` which is sometimes used; Xamarin.Forms has no built-in commanding support beyond the `ICommand` interface definition. You can include a variety of MVVM frameworks to add the necessary base classes to implement MVVM.

#### INotifyPropertyChanged and INotifyCollectionChanged

Both interfaces are fully supported in Xamarin.Forms bindings. Unlike many XAML-based frameworks, property change notifications can be raised on background threads in Xamarin.Forms (just like WPF) and the binding engine will properly transition to the UI thread.

In addition, both environments support `SynchronziationContext` and `async`/`await` to do proper thread marshalling. WPF includes the `Dispatcher` class on all visual elements, Xamarin.Forms has a static method `Device.BeginInvokeOnMainThread` which can also be used (although `SynchronizationContext` is preferred for cross-platform coding).

- Xamarin.Forms includes an `ObservableCollection<T>` which supports collection change notifications.
- You can use `BindingBase.EnableCollectionSynchronization` to enable cross-thread updates for a collection. The API is slightly different from the WPF variation, [check the docs for usage details](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*).

## Data Templates

Data templates are supported in Xamarin.Forms to customize the rendering of a `ListView` row (cell). Unlike WPF which can utilize `DataTemplate`s for any content-oriented control, Xamarin.Forms currently only uses them for `ListView`. The template definition can be defined inline (assigned to the `ItemTemplate` property), or as a resource in a `ResourceDictionary`.

In addition, they are not quite as flexible as their WPF counterpart.

1. The root element of the `DataTemplate` must _always_ be a `ViewCell` object.
2. Data Triggers are fully supported in a Data Template, but must include a `DataType` property indicating the type of the property that the trigger is associated with.
3. `DataTemplateSelector` is also supported, but derives from `DataTemplate` and is therefore just assigned directly to the `ItemTemplate` property (vs. `ItemTemplateSelector` in WPF).

## ItemsControl

There is no built-in equivalent to an `ItemsControl` in Xamarin.Forms; but there is a [custom one for Xamarin.Forms available here](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## User Controls

In WPF, `UserControl`s are used to provide a section of UI which has associated behavior. In Xamarin.Forms, we use the `ContentView` for the same purpose. Both support binding and inclusion in XAML.

## Navigation

WPF includes a rarely used `NavigationService` which could be used to provide a "browser-like" navigation feature. Most apps didn't bother with this however and instead used different `Window` elements, or different sections of the window to display data.

On phone devices, different _screens_ are often the solution and so Xamarin.Forms includes support for several forms of navigation:

| Navigation Style | Page Type |
|--- |--- |
|Stack-based (push/pop)|NavigationPage|
|Master/Detail|MasterDetailPage|
|Tabs|TabbedPage|
|Swipe Left/Right|CarouselView|

The `NavigationPage` is the most common approach, and every page has a `Navigation` property which can be used to push or pop pages on and off the navigation stack. This is the closest equivalent to the `NavigationService` found in WPF.

### URL navigation

WPF is a desktop-oriented technology and can accept command-line parameters to direct startup behavior. Xamarin.Forms can use [deep URL linking](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) to jump to a page on startup.
