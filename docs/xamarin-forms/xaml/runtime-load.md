---
title: "Loading XAML at Runtime in Xamarin.Forms"
description: "XAML can be loaded and parsed at runtime with the LoadFromXaml extension methods."
ms.service: xamarin
ms.assetid: 25F73FBF-2DD3-468E-A2D8-0897414F0F4A
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/12/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Loading XAML at Runtime in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)

The [`Xamarin.Forms.Xaml`](xref:Xamarin.Forms.Xaml) namespace includes two [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) extension methods that can be used to load, and parse XAML at runtime.

## Background

When a Xamarin.Forms XAML class is constructed, the [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) method is indirectly called. This occurs because the code-behind file for a XAML class calls the `InitializeComponent` method from its constructor:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();
    }
}
```

When Visual Studio builds a project containing a XAML file, it parses the XAML file to generate a C# code file (for example, **MainPage.xaml.g.cs**) that contains the definition of the `InitializeComponent` method:

```csharp
private void InitializeComponent()
{
    global::Xamarin.Forms.Xaml.Extensions.LoadFromXaml(this, typeof(MainPage));
    ...
}
```

The `InitializeComponent` method calls the [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) method to extract the XAML file (or its compiled binary) from the .NET Standard library. After extraction, it initializes all of the objects defined in the XAML file, connects them all together in parent-child relationships, attaches event handlers defined in code to events set in the XAML file, and sets the resultant tree of objects as the content of the page.

## Loading XAML at runtime

The [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) methods are `public`, and therefore can be called from Xamarin.Forms applications to load, and parse XAML at runtime. This permits scenarios such as an application downloading XAML from a web service, creating the required view from the XAML, and displaying it in the application.

> [!WARNING]
> Loading XAML at runtime has a significant performance cost, and generally should be avoided.

The following code example shows a simple usage:

```csharp
using Xamarin.Forms.Xaml;
...

string navigationButtonXAML = "<Button Text=\"Navigate\" />";
Button navigationButton = new Button().LoadFromXaml(navigationButtonXAML);
...
_stackLayout.Children.Add(navigationButton);
```

In this example, a [`Button`](xref:Xamarin.Forms.Button) instance is created, with its [`Text`](xref:Xamarin.Forms.Button.Text) property value being set from the XAML defined in the `string`. The `Button` is then added to a [`StackLayout`](xref:Xamarin.Forms.StackLayout) that has been defined in the XAML for the page.

> [!NOTE]
> The [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) extension methods allow a generic type argument to be specified. However, it's rarely necessary to specify the type argument, as it will be inferred from the type of the instance its operating on.

The [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) method can be used to inflate any XAML, with the following example inflating a [`ContentPage`](xref:Xamarin.Forms.ContentPage) and then navigating to it:

```csharp
using Xamarin.Forms.Xaml;
...

// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n</ContentPage>";

ContentPage page = new ContentPage().LoadFromXaml(pageXAML);
await Navigation.PushAsync(page);
```

## Accessing elements

Loading XAML at runtime with the [`LoadFromXaml`](xref:Xamarin.Forms.Xaml.Extensions.LoadFromXaml*) method does not permit strongly-typed access to the XAML elements that have specified runtime object names (using `x:Name`). However, these XAML elements can be retrieved using the [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) method, and then accessed as required:

```csharp
// See the sample for the full XAML string
string pageXAML = "<?xml version=\"1.0\" encoding=\"utf-8\"?>\r\n<ContentPage xmlns=\"http://xamarin.com/schemas/2014/forms\"\nxmlns:x=\"http://schemas.microsoft.com/winfx/2009/xaml\"\nx:Class=\"LoadRuntimeXAML.CatalogItemsPage\"\nTitle=\"Catalog Items\">\n<StackLayout>\n<Label x:Name=\"monkeyName\"\n />\n</StackLayout>\n</ContentPage>";
ContentPage page = new ContentPage().LoadFromXaml(pageXAML);

Label monkeyLabel = page.FindByName<Label>("monkeyName");
monkeyLabel.Text = "Seated Monkey";
...
```

In this example, the XAML for a [`ContentPage`](xref:Xamarin.Forms.ContentPage) is inflated. This XAML includes a [`Label`](xref:Xamarin.Forms.Label) named `monkeyName`, which is retrieved using the [`FindByName`](xref:Xamarin.Forms.NameScopeExtensions.FindByName*) method, before its [`Text`](xref:Xamarin.Forms.Label.Text) property is set.

## Related links

- [LoadRuntimeXAML (sample)](/samples/xamarin/xamarin-forms-samples/xaml-loadruntimexaml)