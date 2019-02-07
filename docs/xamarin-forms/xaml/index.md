---
title: "eXtensible Application Markup Language (XAML)"
description: "XAML is a declarative markup language that can be used to define user interfaces. The user interface is defined in an XML file using the XAML syntax, while runtime behavior is defined in a separate code-behind file."
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
---

# eXtensible Application Markup Language (XAML)

_XAML is a declarative markup language that can be used to define user interfaces. The user interface is defined in an XML file using the XAML syntax, while runtime behavior is defined in a separate code-behind file._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Evolve 2016: Becoming a XAML Master**

> [!NOTE]
> Try out the [XAML Standard Preview](standard/index.md)

<a name="xaml" />

## [XAML Basics](xaml-basics/index.md)

XAML allows developers to define user interfaces in Xamarin.Forms applications using markup rather than code. XAML is never required in a Xamarin.Forms program but it is toolable, and is often more visually coherent and more succinct than equivalent code. XAML is particularly well suited for use with the popular Model-View-ViewModel (MVVM) application architecture: XAML defines the View that is linked to ViewModel code through XAML-based data bindings.

## [XAML Compilation](xamlc.md)

XAML can be optionally compiled directly into intermediate language (IL) with the XAML compiler (XAMLC). This articles describes how to use XAMLC, and its benefits.

## [XAML Previewer](xaml-previewer.md)

The [XAML Previewer](~/xamarin-forms/xaml/xaml-previewer.md) renders a live preview of a page side-by-side with the XAML markup, allowing you to see your user interface rendered as you type.

## [XAML Namespaces](namespaces.md)

XAML uses the `xmlns` XML attribute for namespace declarations. This article introduces the XAML namespace syntax, and demonstrates how to declare a XAML namespace to access a type.

## [XAML Custom Namespace Schemas](custom-namespace-schemas.md)

A XAML custom namespace schema can be defined with the `XmlnsDefinitionAttribute` class, which specifies a mapping between a custom URL and one or more CLR namespaces. The custom namespace schema can then be used in XAML namespace declarations.

## [XAML Markup Extensions](markup-extensions/index.md)

XAML includes markup extensions for setting attributes to values or objects beyond what can be expressed with simple strings. These include referencing constants, static properties and fields, resource dictionaries, and data bindings.

## [Field Modifiers](field-modifiers.md)

The `x:FieldModifier` namespace attribute specifies the access level for generated fields for named XAML elements.

## [Passing Arguments](passing-arguments.md)

XAML can be used to pass arguments to non-default constructors or to factory methods. This article demonstrates using the XAML attributes that can be used to pass arguments to constructors, to call factory methods, and to specify the type of a generic argument.

## [Bindable Properties](bindable-properties.md)

In Xamarin.Forms, the functionality of common language runtime (CLR) properties is extended by bindable properties. A bindable property is a special type of property, where the property's value is tracked by the Xamarin.Forms property system. This article provides an introduction to bindable properties, and demonstrates how to create and consume them.

## [Attached Properties](attached-properties.md)

An attached property is a special type of bindable property, defined in one class but attached to other objects, and recognizable in XAML as an attribute that contains a class and a property name separated by a period. This article provides an introduction to attached properties, and demonstrates how to create and consume them.

## [Resource Dictionaries](resource-dictionaries.md)

XAML resources are definitions of objects that can be used more than once. A [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) allows resources to be defined in a single location, and re-used throughout a Xamarin.Forms application. This article demonstrates how to create and consume a `ResourceDictionary`, and how to merge one `ResourceDictionary` into another.

## [Loading XAML at Runtime](runtime-load.md)

XAML can be loaded and parsed at runtime with the [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) extension methods.
