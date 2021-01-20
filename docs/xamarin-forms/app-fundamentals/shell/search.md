---
title: "Xamarin.Forms Shell Search"
description: "Xamarin.Forms Shell applications can use integrated search functionality that's provided by a search box that can be added to the top of each page."
ms.prod: xamarin
ms.assetid: F8F9471D-6771-4D23-96C0-2B79473A06D4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/21/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shell Search

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms Shell includes integrated search functionality that's provided by the `SearchHandler` class. Search capability can be added to a page by setting the `Shell.SearchHandler` attached property to a sub-classed `SearchHandler` object. This results in a search box being added at the top of the page:

[![Screenshot of a Shell SearchHandler, on iOS and Android](search-images/searchhandler.png "Shell SearchHandler")](search-images/searchhandler-large.png#lightbox "Shell SearchHandler")

When a query is entered into the search box, the `Query` property is updated, and on each update the `OnQueryChanged` method is executed. This method can be overridden to populate the search suggestions area with data:

[![Screenshot of a search results in a Shell SearchHandler, on iOS and Android](search-images/search-suggestions.png "Shell SearchHandler search results")](search-images/search-suggestions-large.png#lightbox "Shell SearchHandler search results")

Then, when a result is selected from the search suggestions area, the `OnItemSelected` method is executed. This method can be overridden to respond appropriately, such as by navigating to a detail page.

## Create a SearchHandler

Search functionality can be added to a Shell application by subclassing the `SearchHandler` class, and overriding the `OnQueryChanged` and `OnItemSelected` methods:

```csharp
public class MonkeySearchHandler : SearchHandler
{
    protected override void OnQueryChanged(string oldValue, string newValue)
    {
        base.OnQueryChanged(oldValue, newValue);

        if (string.IsNullOrWhiteSpace(newValue))
        {
            ItemsSource = null;
        }
        else
        {
            ItemsSource = MonkeyData.Monkeys
                .Where(monkey => monkey.Name.ToLower().Contains(newValue.ToLower()))
                .ToList<Animal>();
        }
    }

    protected override async void OnItemSelected(object item)
    {
        base.OnItemSelected(item);

        // Note: strings will be URL encoded for navigation (e.g. "Blue Monkey" becomes "Blue%20Monkey"). Therefore, decode at the receiver.
        await (App.Current.MainPage as Xamarin.Forms.Shell).GoToAsync($"monkeydetails?name={((Animal)item).Name}");
    }
}
```

The `OnQueryChanged` override has two arguments: `oldValue`, which contains the previous search query, and `newValue`, which contains the current search query. The search suggestions area can be updated by setting the `SearchHandler.ItemsSource` property to an `IEnumerable` collection that contains items that match the current search query.

When a search result is selected by the user, the `OnItemSelected` override is executed and the `SelectedItem` property is set. In this example, the method navigates to another page that displays data about the selected `Animal`. For more information about navigation, see [Xamarin.Forms Shell Navigation](navigation.md).

> [!NOTE]
> Additional `SearchHandler` properties can be set to control the search box appearance.

## Consume a SearchHandler

The subclassed `SearchHandler` can be consumed by setting the `Shell.SearchHandler` attached property to an object of the subclassed type:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true"
                                      DisplayMemberName="Name" />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

The equivalent C# code is:

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name"
});
```

The `MonkeySearchHandler.OnQueryChanged` method returns a `List` of `Animal` objects. The `DisplayMemberName` property is set to the `Name` property of each `Animal` object, and so the data displayed in the suggestions area will be each animal name.

The `ShowsResults` property is set to `true`, so that search suggestions are displayed as the user enters a search query:

[![Screenshot of search results in a Shell SearchHandler, on iOS and Android, with results for the partial string M.](search-images/search-results.png "Shell SearchHandler search results")](search-images/search-results-large.png#lightbox "Shell SearchHandler search results")

As the search query changes, the search suggestions area is updated:

[![Screenshot of search results in a Shell SearchHandler, on iOS and Android, with results for the partial string M o n.](search-images/search-results-change.png "Shell SearchHandler search results")](search-images/search-results-change-large.png#lightbox "Shell SearchHandler search results")

When a search result is selected, the `MonkeyDetailPage` is navigated to, and data about the selected monkey is displayed:

[![Screenshot of monkey details, on iOS and Android](search-images/detailpage.png "Monkey details")](search-images/detailpage-large.png#lightbox "Monkey details")

## Define search results item appearance

In addition to displaying `string` data in the search results, the appearance of each search result item can be defined by setting the `SearchHandler.ItemTemplate` property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">    
    <Shell.SearchHandler>
        <controls:MonkeySearchHandler Placeholder="Enter search term"
                                      ShowsResults="true">
            <controls:MonkeySearchHandler.ItemTemplate>
                <DataTemplate>
                    <Grid Padding="10">
                        <Grid.ColumnDefinitions>
                            <ColumnDefinition Width="0.15*" />
                            <ColumnDefinition Width="0.85*" />
                        </Grid.ColumnDefinitions>
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="40"
                               WidthRequest="40" />
                        <Label Grid.Column="1"
                               Text="{Binding Name}"
                               FontAttributes="Bold" />
                    </Grid>
                </DataTemplate>
            </controls:MonkeySearchHandler.ItemTemplate>
       </controls:MonkeySearchHandler>
    </Shell.SearchHandler>
    ...
</ContentPage>
```

The equivalent C# code is:

```csharp
Shell.SetSearchHandler(this, new MonkeySearchHandler
{
    Placeholder = "Enter search term",
    ShowsResults = true,
    DisplayMemberName = "Name",
    ItemTemplate = new DataTemplate(() =>
    {
        Grid grid = new Grid { Padding = 10 };
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.15, GridUnitType.Star) });
        grid.ColumnDefinitions.Add(new ColumnDefinition { Width = new GridLength(0.85, GridUnitType.Star) });

        Image image = new Image { Aspect = Aspect.AspectFill, HeightRequest = 40, WidthRequest = 40 };
        image.SetBinding(Image.SourceProperty, "ImageUrl");
        Label nameLabel = new Label { FontAttributes = FontAttributes.Bold };
        nameLabel.SetBinding(Label.TextProperty, "Name");

        grid.Children.Add(image);
        grid.Children.Add(nameLabel, 1, 0);
        return grid;
    })
});
```

The elements specified in the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) define the appearance of each item in the suggestions area. In this example, layout within the `DataTemplate` is managed by a [`Grid`](xref:Xamarin.Forms.Grid). The `Grid` contains an [`Image`](xref:Xamarin.Forms.Image) object, and a [`Label`](xref:Xamarin.Forms.Label) object, that both bind to properties of each `Monkey` object.

The following screenshots show the result of templating each item in the suggestions area:

[![Screenshot of templated search results in a Shell SearchHandler, on iOS and Android](search-images/search-results-template.png "Shell SearchHandler templated search results")](search-images/search-results-template-large.png#lightbox "Shell SearchHandler templated search results")

For more information about data templates, see [Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

## Search box visibility

When a `SearchHandler` is added at the top of a page, by default the search box is visible and fully expanded. However, this behavior can be changed by setting the `SearchHandler.SearchBoxVisibility` property to one of the `SearchBoxVisibility` enumeration members:

- `Hidden` – the search box is not visible or accessible.
- `Collapsible` – the search box is hidden until the user performs an action to reveal it. On iOS the search box is revealed by vertically bouncing the page content, and on Android the search box is revealed by tapping the question mark icon.
- `Expanded` – the search box is visible and fully expanded. This is the default value of the `SearchHandler.SearchBoxVisibility` property.

> [!IMPORTANT]
> On iOS, a collapsible search box requires iOS 11 or greater.

The following example shows to how to hide the search box:

```xaml
<ContentPage ...
             xmlns:controls="clr-namespace:Xaminals.Controls">
    <Shell.SearchHandler>
        <controls:AnimalSearchHandler SearchBoxVisibility="Hidden"
                                      ... />
    </Shell.SearchHandler>
    ...
</ContentPage>
```

## Search box focus

Tapping in a search box invokes the onscreen keyboard, with the search box gaining input focus. This can also be achieved programmatically by calling the `Focus` method, which attempts to set input focus on the search box, and returns `true` if successful. When a search box gains focus, the `Focus` event is fired and the overridable `OnFocused` method is called.

When a search box has input focus, tapping elsewhere on the screen dismisses the onscreen keyboard, and the search box loses input focus. This can also be achieved programmatically by calling the `Unfocus` method. When a search box loses focus, the `Unfocused` event is fired and the overridable `OnUnfocus` method is called.

The focus state of a search box can be retrieved through the `IsFocused` property, which returns `true` if a `SearchHandler` currently has input focus.

## SearchHandler appearance

The `SearchHandler` class defines the following properties that affect its appearance:

- `BackgroundColor`, of type `Color`, is the color of the background to the search box text.
- `CancelButtonColor`, of type `Color`, is the color of the cancel button.
- `CharacterSpacing`, of type `double`, is the spacing between characters of the `SearchHandler` text.
- `FontAttributes`, of type `FontAttributes`, indicates if the search box text is italic or bold.
- `FontFamily`, of type `string`, is the font family used for the search box text.
- `FontSize`, of type `double`, is the size of the search box text.
- `HorizontalTextAlignment`, of type `TextAlignment`, is the horizontal alignment of the search box text.
- `PlaceholderColor`, of type `Color`, is the color of the placeholder search box text.
- `TextColor`, of type `Color`, is the color of the search box text.
- `TextTransform`, of type `TextTransform`, determines the casing of the search box text.
- `VerticalTextAlignment`, of type `TextAlignment`, is the vertical alignment of the search box text.

## SearchHandler keyboard

The keyboard that's presented when users interact with a `SearchHandler` can be set programmatically via the `Keyboard` property, to one of the following properties from the [`Keyboard`](xref:Xamarin.Forms.Keyboard) class:

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) – used for texting and places where emoji are useful.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) – the default keyboard.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) – used when entering email addresses.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) – used when entering numbers.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) – used when entering text, without any [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) specified.
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) – used when entering telephone numbers.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) – used when entering text.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) – used for entering file paths & web addresses.

This can be accomplished in XAML as follows:

```xaml
<SearchHandler Keyboard="Email" />
```

The equivalent C# code is:

```csharp
SearchHandler searchHandler = new SearchHandler { Keyboard = Keyboard.Email };
```

The [`Keyboard`](xref:Xamarin.Forms.Keyboard) class also has a [`Create`](xref:Xamarin.Forms.Keyboard.Create*) factory method that can be used to customize a keyboard by specifying capitalization, spellcheck, and suggestion behavior. [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) enumeration values are specified as arguments to the method, with a customized `Keyboard` being returned. The `KeyboardFlags` enumeration contains the following values:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) – no features are added to the keyboard.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) – indicates that the first letter of the first word of each entered sentence will be automatically capitalized.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) – indicates that spellcheck will be performed on entered text.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) – indicates that word completions will be offered on entered text.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) – indicates that the first letter of each word will be automatically capitalized.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) – indicates that every character will be automatically capitalized.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) – indicates that no automatic capitalization will occur.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) – indicates that spellcheck, word completions, and sentence capitalization will occur on entered text.

The following XAML code example shows how to customize the default [`Keyboard`](xref:Xamarin.Forms.Keyboard) to offer word completions and capitalize every entered character:

```xaml
<SearchHandler Placeholder="Enter search terms">
    <SearchHandler.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </SearchHandler.Keyboard>
</SearchHandler>
```

The equivalent C# code is:

```csharp
SearchHandler searchHandler = new SearchHandler { Placeholder = "Enter search terms" };
searchHandler.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

## SearchHandler reference

The `SearchHandler` class defines the following properties that control its appearance and behavior:

- `BackgroundColor`, of type `Color`, is the color of the background to the search box text.
- `CancelButtonColor`, of type `Color`, is the color of the cancel button.
- `ClearIcon`, of type [`ImageSource`](xref:Xamarin.Forms.ImageSource), the icon displayed to clear the contents of the search box.
- `ClearIconHelpText`, of type `string`, the accessible help text for the clear icon.
- `ClearIconName`, of type `string`, the name of the clear icon for use with screen readers.
- `ClearPlaceholderCommand`, of type `ICommand`, which is executed when the `ClearPlaceholderIcon` is tapped.
- `ClearPlaceholderCommandParameter`, of type `object`, which is the parameter that's passed to the `ClearPlaceholderCommand`.
- `ClearPlaceholderEnabled`, of type `bool`, which determines whether the `ClearPlaceholderCommand` can be executed. The default value is `true`.
- `ClearPlaceholderHelpText`, of type `string`, the accessible help text for the clear placeholder icon.
- `ClearPlaceholderIcon`, of type [`ImageSource`](xref:Xamarin.Forms.ImageSource), the clear placeholder icon displayed when the search box is empty.
- `ClearPlaceholderName`, of type `string`, the name of the clear placeholder icon for use with screen readers.
- `Command`, of type `ICommand`, which is executed when the search query is confirmed.
- `CommandParameter`, of type `object`, which is the parameter that's passed to the `Command`.
- `DisplayMemberName`, of type `string`, representing the name or path of the property that's displayed for each item of data in the `ItemsSource` collection.
- `FontAttributes`, of type `FontAttributes`, indicates if the search box text is italic or bold.
- `FontFamily`, of type `string`, is the font family used for the search box text.
- `FontSize`, of type `double`, is the size of the search box text.
- `HorizontalTextAlignment`, of type `TextAlignment`, is the horizontal alignment of the search box text.
- `IsFocused`, of type `bool`, representing whether a `SearchHandler` currently has input focus.
- `IsSearchEnabled`, of type `bool`, representing the enabled state of the search box. The default value is `true`.
- `ItemsSource`, of type `IEnumerable`, specifies the collection of items to be displayed in the suggestion area, and has a default value of `null`.
- `ItemTemplate`, of type [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), specifies the template to apply to each item in the collection of items to be displayed in the suggestion area.
- `Keyboard`, of type `Keyboard`, is the keyboard for the `SearchHandler`.
- `Placeholder`, of type `string`, the text to display when the search box is empty.
- `PlaceholderColor`, of type `Color`, is the color of the placeholder search box text.
- `Query`, of type `string`, the user entered text in the search box.
- `QueryIcon`, of type [`ImageSource`](xref:Xamarin.Forms.ImageSource), the icon used to indicate to the user that search is available.
- `QueryIconHelpText`, of type `string`, the accessible help text for the query icon.
- `QueryIconName`, of type `string`, the name of the query icon for use with screen readers.
- `SearchBoxVisibility`, of type `SearchBoxVisibility`, the visibility of the search box. By default, the search box is visible and fully expanded.
- `SelectedItem`, of type `object`, the selected item in the search results. This property is read only, and has a default value of `null`.
- `ShowsResults`, of type `bool`, indicates whether search results should be expected in the suggestion area, on text entry. The default value is `false`.
- `TextColor`, of type `Color`, is the color of the search box text.
- `TextTransform`, of type `TextTransform`, determines the casing of the search box text.

All of these properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that the properties can be targets of data bindings.

In addition, the `SearchHandler` class provides the following overridable methods:

- `OnClearPlaceholderClicked`, that's called whenever the `ClearPlaceholderIcon` is tapped.
- `OnItemSelected`, that's called whenever a search result is selected by the user.
- `OnFocused`, that's called when a `SearchHandler` acquires input focus.
- `OnQueryChanged`, that's called when the `Query` property changes.
- `OnQueryConfirmed`, that's called whenever the user presses enter or confirms their query in the search box.
- `OnUnfocus`, that's called when a `SearchHandler` loses input focus.

## Related links

- [Xaminals (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Xamarin.Forms Shell Navigation](navigation.md)