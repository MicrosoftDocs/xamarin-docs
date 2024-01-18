---
title: "Xamarin.Forms SearchBar"
description: "The Xamarin.Forms SearchBar is a user input control that is used for initiating a search. The SearchBar control supports placeholder text, query input, execution, and cancellation. This article explains how to use a SearchBar in XAML and code."
ms.service: xamarin
ms.assetId: F5EFEA72-CB23-4DD6-9545-D9BB755AF3CB
ms.subservice: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/21/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms SearchBar

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)

The Xamarin.Forms [`SearchBar`](xref:Xamarin.Forms.SearchBar) is a user input control used to initiating a search. The `SearchBar` control supports placeholder text, query input, search execution, and cancellation. The following screenshot shows a `SearchBar` query with results displayed in a `ListView`:

[![Screenshot of SearchBar on iOS and Android](searchbar-images/device-searchbars-cropped.png "SearchBar on iOS and Android")](searchbar-images/device-searchbars.png#lightbox "SearchBar on iOS and Android")

The `SearchBar` class defines the following properties:

* [`CancelButtonColor`](xref:Xamarin.Forms.SearchBar.CancelButtonColor) is a `Color` that defines the color of the cancel button.
* `CharacterSpacing`, of type `double`, is the spacing between characters of the `SearchBar` text.
* [`FontAttributes`](xref:Xamarin.Forms.SearchBar.FontAttributes) is a `FontAttributes` enum value that determines whether the `SearchBar` font is bold, italic, or neither.
* [`FontFamily`](xref:Xamarin.Forms.SearchBar.FontFamily) is a `string` that determines the font family used by the `SearchBar`.
* [`FontSize`](xref:Xamarin.Forms.SearchBar.FontSize) can be a `NamedSize` enum value or a `double` value that represents specific font sizes across platforms.
* [`HorizontalTextAlignment`](xref:Xamarin.Forms.SearchBar.HorizontalTextAlignment) is a `TextAlignment` enum value that defines the horizontal alignment of the query text.
* `VerticalTextAlignment` is a `TextAlignment` enum value that defines the vertical alignment of the query text.
* [`Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) is a `string` that defines the placeholder text, such as "Search...".
* [`PlaceholderColor`](xref:Xamarin.Forms.InputView.PlaceholderColor) is a `Color` that defines the color of the placeholder text.
* [`SearchCommand`](xref:Xamarin.Forms.SearchBar.SearchCommand) is an `ICommand` that allows binding user actions, such as finger taps or clicks, to commands defined on a viewmodel.
* [`SearchCommandParameter`](xref:Xamarin.Forms.SearchBar.SearchCommandParameter) is an `object` that specifies the parameter that should be passed to the `SearchCommand`.
* [`Text`](xref:Xamarin.Forms.InputView.Text) is a `string` containing the query text in the `SearchBar`.
* [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) is a `Color` that defines the query text color.
* `TextTransform` is a `TextTransform` value that determines the casing of the `SearchBar` text.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means the `SearchBar` can be customized and be the target of data bindings. Specifying font properties on the `SearchBar` is consistent with customizing text on other [Xamarin.Forms Text controls](~/xamarin-forms/user-interface/text/index.md). For more information, see [Fonts in Xamarin.Forms](~/xamarin-forms/user-interface/text/fonts.md).

## Create a SearchBar

A `SearchBar` can be instantiated in XAML. Its optional `Placeholder` property can be set to define the hint text in the query input box. The default value for the `Placeholder` is an empty string so no placeholder will appear if it isn't set. The following example shows how to instantiate a `SearchBar` in XAML with the optional `Placeholder` property set:

```xaml
<SearchBar Placeholder="Search items..." />
```

A `SearchBar` can also be created in code:

```csharp
SearchBar searchBar = new SearchBar{ Placeholder = "Search items..." };
```

### SearchBar appearance properties

The `SearchBar` control defines many properties that customize the appearance of the control. The following example shows how to instantiate a `SearchBar` in XAML with multiple properties specified:

```xaml
<SearchBar Placeholder="Search items..."
           CancelButtonColor="Orange"
           PlaceholderColor="Orange"
           TextColor="Orange"
           TextTransform="Lowercase"
           HorizontalTextAlignment="Center"
           FontSize="Medium"
           FontAttributes="Italic" />
```

These properties can also be specified when creating a `SearchBar` object in code:

```csharp
SearchBar searchBar = new SearchBar
{
    Placeholder = "Search items...",
    PlaceholderColor = Color.Orange,
    TextColor = Color.Orange,
    TextTransform = TextTransform.Lowercase,
    HorizontalTextAlignment = TextAlignment.Center,
    FontSize = Device.GetNamedSize(NamedSize.Medium, typeof(SearchBar)),
    FontAttributes = FontAttributes.Italic
};
```

The following screenshot shows the resulting `SearchBar` control:

[![Screenshot of customized SearchBar on iOS and Android](searchbar-images/device-searchbars-styled-cropped.png "Customized SearchBar on iOS and Android")](searchbar-images/device-searchbars-styled.png#lightbox "Customized SearchBar on iOS and Android")

> [!NOTE]
> On iOS, the `SearchBarRenderer` class contains an overridable `UpdateCancelButton` method. This method controls when the cancel button appears, and can be overridden in a custom renderer. For more information about custom renderers, see [Xamarin.Forms Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## Perform a search with event handlers

A search can be executed using the `SearchBar` control by attaching an event handler to one of the following events:

* [`SearchButtonPressed`](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) is called when the user either clicks the search button or presses the enter key.
* [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) is called anytime the text in the query box is changed.

The following example shows an event handler attached to the `TextChanged` event in XAML and uses a `ListView` to display search results:

```xaml
<SearchBar TextChanged="OnTextChanged" />
<ListView x:Name="searchResults" >
```

An event handler can also be attached to a `SearchBar` created in code:

```csharp
SearchBar searchBar = new SearchBar {/*...*/};
searchBar.TextChanged += OnTextChanged;
```

The `TextChanged` event handler in the code-behind file is the same, whether the `SearchBar` is created via XAML or code:

```csharp
void OnTextChanged(object sender, EventArgs e)
{
    SearchBar searchBar = (SearchBar)sender;
    searchResults.ItemsSource = DataService.GetSearchResults(searchBar.Text);
}
```

The previous example implies the existence of a `DataService` class with a `GetSearchResults` method capable of returning items that match a query. The `SearchBar` control's `Text` property value is passed to the `GetSearchResults` method and the result is used to update the `ListView` control's `ItemsSource` property. The overall effect is that search results are displayed in the `ListView` control.

The sample application provides a `DataService` class implementation that can be used to test search functionality.

## Perform a search using a viewmodel

A search can be executed without event handlers by binding the `SearchCommand` and `SearchCommandParameter` properties to `ICommand` implementations. The sample project demonstrates these implementations using the Model-View-ViewModel (MVVM) pattern. For more information about data bindings with MVVM, see [Data Bindings with MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md).

The viewmodel in the sample application contains the following code:

```csharp
public class SearchViewModel : INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void NotifyPropertyChanged([CallerMemberName] string propertyName = "")
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }

    public ICommand PerformSearch => new Command<string>((string query) =>
    {
        SearchResults = DataService.GetSearchResults(query);
    });

    private List<string> searchResults = DataService.Fruits;
    public List<string> SearchResults
    {
        get
        {
            return searchResults;
        }
        set
        {
            searchResults = value;
            NotifyPropertyChanged();
        }
    }
}
```

> [!NOTE]
> The viewmodel assumes the existence of a `DataService` class capable of performing searches. The `DataService` class, including example data, is available in the sample application.

The following XAML shows how to bind a `SearchBar` to the example viewmodel, with a `ListView` control displaying the search results:

```xaml
<ContentPage ...>
    <ContentPage.BindingContext>
        <viewmodels:SearchViewModel />
    </ContentPage.BindingContext>
    <StackLayout ...>
        <SearchBar x:Name="searchBar"
                   ...
                   SearchCommand="{Binding PerformSearch}"
                   SearchCommandParameter="{Binding Text, Source={x:Reference searchBar}}"/>
        <ListView x:Name="searchResults"
                  ...
                  ItemsSource="{Binding SearchResults}" />
    </StackLayout>
</ContentPage>
```

This example sets the `BindingContext` to be an instance of the `SearchViewModel` class. It binds the `SearchCommand` property to the `PerformSearch` `ICommand` in the viewmodel, and binds the `SearchBar` `Text` property to the `SearchCommandParameter` property. The `ListView.ItemsSource` property is bound to the `SearchResults` property of the viewmodel.

For more information about the `ICommand` Interface and bindings, see [Xamarin.Forms data binding](~/xamarin-forms/app-fundamentals/data-binding/index.md) and [the ICommand interface](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## Related links

* [SearchBar Demos](/samples/xamarin/xamarin-forms-samples/userinterface-searchbardemos/)
* [Xamarin.Forms Text controls](~/xamarin-forms/user-interface/text/index.md)
* [Fonts in Xamarin.Forms](~/xamarin-forms/user-interface/text/fonts.md)
* [Xamarin.Forms data binding](~/xamarin-forms/app-fundamentals/data-binding/index.md)