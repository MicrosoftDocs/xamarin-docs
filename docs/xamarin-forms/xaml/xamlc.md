---
title: "XAML Compilation in Xamarin.Forms"
description: "This article explains how XAML can be optionally compiled directly into intermediate language (IL) with the Xamarin.Forms XAML compiler (XAMLC)."
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/03/2021
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# XAML Compilation in Xamarin.Forms

_XAML can be optionally compiled directly into intermediate language (IL) with the XAML compiler (XAMLC)._

XAML compilation offers a number of a benefits:

- It performs compile-time checking of XAML, notifying the user of any errors.
- It removes some of the load and instantiation time for XAML elements.
- It helps to reduce the file size of the final assembly by no longer including .xaml files.

XAML compilation is disabled by default in the framework. However, it's enabled in the templates for new projects. It can be explicitly enabled or disabled (`XamlCompilationOptions.Skip`) at both the assembly and class level by adding the [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) attribute.

The following code example demonstrates enabling XAML compilation at the assembly level:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

While the attribute can be placed anywhere, a good place to put it is in **AssemblyInfo.cs**.

In this example, compile-time checking of all the XAML contained within the assembly will be performed, with XAML errors being reported at compile-time rather than run-time. Therefore, the `assembly` prefix to the [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) attribute specifies that the attribute applies to the entire assembly.

> [!NOTE]
> The [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) attribute and the [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions) enumeration reside in the `Xamarin.Forms.Xaml` namespace, which must be imported to use them.

The following code example demonstrates enabling XAML compilation at the class level:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

In this example, compile-time checking of the XAML for the `HomePage` class will be performed and errors reported as part of the compilation process.

> [!NOTE]
> Compiled bindings can be enabled to improve data binding performance in Xamarin.Forms applications. For more information, see [Compiled Bindings](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

## Related Links

- [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
