---
title: "XAML Namespaces in Xamarin.Forms"
description: "XAML uses the xmlns XML attribute for namespace declarations. This article introduces the XAML namespace syntax, and demonstrates how to declare a XAML namespace to access a type."
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# XAML Namespaces in Xamarin.Forms

_XAML uses the xmlns XML attribute for namespace declarations. This article introduces the XAML namespace syntax, and demonstrates how to declare a XAML namespace to access a type._

## Overview

There are two XAML namespace declarations that are always within the root element of a XAML file. The first defines the default namespace, as shown in the following XAML code example:

```xaml
xmlns="http://xamarin.com/schemas/2014/forms"
```

The default namespace specifies that elements defined within the XAML file with no prefix refer to Xamarin.Forms classes, such as [`ContentPage`](xref:Xamarin.Forms.ContentPage).

The second namespace declaration uses the `x` prefix, as shown in the following XAML code example:

```xaml
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML uses prefixes to declare non-default namespaces, with the prefix being used when referencing types within the namespace. The `x` namespace declaration specifies that elements defined within the XAML with a prefix of `x` are used for elements and attributes that are intrinsic to XAML (specifically the 2009 XAML specification).

The following table outlines the `x` namespace attributes supported by Xamarin.Forms:

|Construct|Description|
|--- |--- |
|`x:Arguments`|Specifies constructor arguments for a non-default constructor, or for a factory method object declaration.|
|`x:Class`|Specifies the namespace and class name for a class defined in XAML. The class name must match the class name of the code-behind file. Note that this construct can only appear in the root element of a XAML file.|
|`x:DataType`|Specifies the type of the object that the XAML element, and it's children, will bind to.|
|`x:FactoryMethod`|Specifies a factory method that can be used to initialize an object.|
|`x:FieldModifier`|Specifies the access level for generated fields for named XAML elements.|
|`x:Key`|Specifies a unique user-defined key for each resource in a `ResourceDictionary`. The key's value is used to retrieve the XAML resource, and is typically used as the argument for the `StaticResource` markup extension.|
|`x:Name`|Specifies a runtime object name for the XAML element. Setting `x:Name` is similar to declaring a variable in code.|
|`x:TypeArguments`|Specifies the generic type arguments to the constructor of a generic type.|

For more information about the `x:DataType` attribute, see [Compiled Bindings](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md). For more information about the `x:FieldModifier` attribute, see [Field Modifiers](~/xamarin-forms/xaml/field-modifiers.md). For more information about the `x:Arguments` and `x:FactoryMethod` attributes, see [Passing Arguments in XAML](~/xamarin-forms/xaml/passing-arguments.md). For more information about the `x:TypeArguments` attribute, see [Generics in XAML with Xamarin.Forms](generics.md).

> [!NOTE]
> In addition to the namespace attributes listed above, Xamarin.Forms also includes markup extensions that can be consumed through the `x` namespace prefix. For more information, see [Consuming XAML Markup Extensions](~/xamarin-forms/xaml/markup-extensions/consuming.md).

In XAML, namespace declarations inherit from parent element to child element. Therefore, when defining a namespace in the root element of a XAML file, all elements within that file inherit the namespace declaration.

## Declaring Namespaces for Types

Types can be referenced in XAML by declaring a XAML namespace with a prefix, with the namespace declaration specifying the Common Language Runtime (CLR) namespace name, and optionally an assembly name. This is achieved by defining values for the following keywords within the namespace declaration:

- **clr-namespace:** or **using:** – the CLR namespace declared within the assembly that contains the types to expose as XAML elements. This keyword is required.
- **assembly=** – the assembly that contains the referenced CLR namespace. This value is the name of the assembly, without the file extension. The path to the assembly should be established as a reference in the project file that contains the XAML file that will reference the assembly. This keyword can be omitted if the **clr-namespace** value is within the same assembly as the application code that's referencing the types.

Note that the character separating the `clr-namespace` or `using` token from its value is a colon, whereas the character separating the `assembly` token from its value is an equal sign. The character to use between the two tokens is a semicolon.

The following code example shows a XAML namespace declaration:

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

Alternatively, this can be written as:

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

The `local` prefix is a convention used to indicate that the types within the namespace are local to the application. Alternatively, if the types are in a different assembly, the assembly name should also be defined in the namespace declaration, as demonstrated in the following XAML code example:

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

The namespace prefix is then specified when declaring an instance of a type from an imported namespace, as demonstrated in the following XAML code example:

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

For information about defining a custom namespace schema, see [XAML Custom Namespace Schemas](custom-namespace-schemas.md).

## Summary

This article introduced the XAML namespace syntax, and demonstrated how to declare a XAML namespace to access a type. XAML uses the `xmlns` XML attribute for namespace declarations, and types can be referenced in XAML by declaring a XAML namespace with a prefix.

## Related Links

- [Passing Arguments in XAML](~/xamarin-forms/xaml/passing-arguments.md)
- [Generics in XAML with Xamarin.Forms](generics.md)
