---
title: "XAML Compilation"
description: "XAML can be optionally compiled directly into intermediate language (IL) with the XAML compiler (XAMLC)."
ms.topic: article
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
---

# XAML Compilation

_XAML can be optionally compiled directly into intermediate language (IL) with the XAML compiler (XAMLC)._

XAMLC offers a number of a benefits:

- It performs compile-time checking of XAML, notifying the user of any errors.
- It removes some of the load and instantiation time for XAML elements.
- It helps to reduce the file size of the final assembly by no longer including .xaml files.

XAMLC is disabled by default to ensure backwards compatibility. It can be enabled at both the assembly and class level by adding the `XamlCompilation` attribute.

The following code example demonstrates enabling XAMLC at the assembly level:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

In this example, compile-time checking of all the XAML contained within the `PhotoApp` namespace will be performed,
with XAML errors being reported at compile-time rather than run-time.
The `assembly` prefix to the `XamlCompilation` attribute specifies that the attribute applies to the entire assembly.

The following code example demonstrates enabling XAMLC at the class level:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

In this example, compile-time checking of the XAML for the `HomePage`
class will be performed and errors reported as part of the compilation process.

> [!NOTE]
> The `XamlCompilation` attribute and the `XamlCompilationOptions` enumeration reside in the `Xamarin.Forms.Xaml` namespace, which must be imported to use them.


## Related Links

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
