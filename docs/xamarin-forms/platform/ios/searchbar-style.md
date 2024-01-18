---
title: "SearchBar Style on iOS"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article explains how to consume the iOS platform-specific that controls whether a SearchBar has a background."
ms.service: xamarin
ms.assetid: 3D512DD6-078E-4BC6-926E-62BA6F4DE640
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# SearchBar style on iOS

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

This iOS platform-specific controls whether a [`SearchBar`](xref:Xamarin.Forms.SearchBar) has a background. It's consumed in XAML by setting the `SearchBar.SearchBarStyle` bindable property to a value of the `UISearchBarStyle` enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ios:SearchBar.SearchBarStyle="Minimal"
                   Placeholder="Enter search term" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SearchBar searchBar = new SearchBar { Placeholder = "Enter search term" };
searchBar.On<iOS>().SetSearchBarStyle(UISearchBarStyle.Minimal);
```

The `SearchBar.On<iOS>` method specifies that this platform-specific will only run on iOS. The `SearchBar.SetSearchBarStyle` method, in the [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) namespace, is used to control whether the [`SearchBar`](xref:Xamarin.Forms.SearchBar) has a background. The `UISearchBarStyle` enumeration provides three possible values:

- `Default` indicates that the [`SearchBar`](xref:Xamarin.Forms.SearchBar) has the default style. This is the default value of the `SearchBar.SearchBarStyle` bindable property.
- `Prominent` indicates that the [`SearchBar`](xref:Xamarin.Forms.SearchBar) has a translucent background, and the search field is opaque.
- `Minimal` indicates that the [`SearchBar`](xref:Xamarin.Forms.SearchBar) has no background, and the search field is translucent.

In addition, the `SearchBar.GetSearchBarStyle` method can be used to return the `UISearchBarStyle` that's applied to the `SearchBar`.

The result is that a specified `UISearchBarStyle` member is applied to a [`SearchBar`](xref:Xamarin.Forms.SearchBar), which controls whether the `SearchBar` has a background:

![Screenshot of SearchBar styles, on iOS](searchbar-style-images/searchbar-styles.png "SearchBar styles on iOS")

The following screenshots show the `UISearchBarStyle` members applied to [`SearchBar`](xref:Xamarin.Forms.SearchBar) objects that have their `BackgroundColor` property set:

![Screenshot of SearchBar styles with background color, on iOS](searchbar-style-images/searchbar-background-styles.png "SearchBar styles with background color on iOS")

## Related links

- [PlatformSpecifics (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)