---
title: "Xamarin.Forms Entry"
description: "This article explains how to use the Xamarin.Forms Entry class to accept single-line text or password input in an application."
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
---

# Xamarin.Forms Entry

_Single-line text or password input_

The Xamarin.Forms `Entry` is used for single-line text input. The `Entry`, like the `Editor` view, supports multiple keyboard types. Additionally, the `Entry` can be used as a password field.

## Display Customization

### Setting and Reading Text

The `Entry`, like other text-presenting views, exposes the `Text` property. This property can be used to set and read the text presented by the `Entry`. The following example demonstrates setting the `Text` property in XAML:

```xaml
<Entry Text="I am an Entry" />
```

In C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

To read text, access the `Text` property in C#:

```csharp
var text = MyEntry.Text;
```

> [!NOTE]
> The width of an `Entry` can be defined by setting its `WidthRequest` property. Do not depend on the width of an `Entry` being defined based on the value of its `Text` property.

### Limiting Input Length

The [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) property can be used to limit the input length that's permitted for the [`Entry`](xref:Xamarin.Forms.Entry). This property should be set to a positive integer:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

A [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) property value of 0 indicates that no input will be allowed, and a value of `int.MaxValue`, which is the default value for an [`Entry`](xref:Xamarin.Forms.Entry), indicates that there is no effective limit on the number of characters that may be entered.

### Keyboards

The keyboard that is presented when users interact with an `Entry` can be set programmatically via the `Keyboard` property.

The options for the keyboard type are:

- **Default** &ndash; the default keyboard
- **Chat** &ndash; used for texting & places where emoji are useful
- **Email** &ndash; used when entering email addresses
- **Numeric** &ndash; used when entering numbers
- **Telephone** &ndash; used when entering telephone numbers
- **Url** &ndash; used for entering file paths and web addresses

The following will create an `Entry` with keyboard type [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat) in XAML:

```xaml
<Entry Keyboard="Chat" />
```

In C#:

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
```

There is an [example of each keyboard](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/choose-keyboard-for-entry/)
in our Recipes section.

For additional keyboard scenarios not covered by the above options, the [`Keyboard`](xref:Xamarin.Forms.Keyboard) class exposes the static method [`Create`](https://docs.microsoft.com/dotnet/api/xamarin.forms.keyboard.create), taking a collection of enum flags [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) and returning a new instance of [`Keyboard`](xref:Xamarin.Forms.Keyboard).

The available enum values are:

- **None** &ndash; no features are added to the keyboard
- **CapitalizeSentence** &ndash; used for capitalizing the first words of sentences
- **Spellcheck** &ndash; used for enabling spellcheck on text entered by the user
- **Suggestions** &ndash; used for enabling suggested word completion on text entered by the user
- **CapitalizeWord** &ndash; used for capitalizing words entered by the user
- **CapitalizeCharacter** &ndash; used for capitalizing each character entered by the user
- **CapitalizeNone** &ndash; no capitalization rules are enforced to the keyboard
- **All** &ndash; used for capitalizing the first words of sentences, performing spellcheck and enabling suggested word completions on text entered by the user

The following will create an Entry in XAML with a custom keyboard that capitalizes the first words of sentences and performs spellcheck:

```xaml
<Entry>
    <Entry.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>CapitalizeSentence,Spellcheck</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Entry.Keyboard>
</Entry>
```

In C#:

```csharp
var entry = new Entry { Keyboard = Keyboard.Create(KeyboardFlags.CapitalizeSentence | KeyboardFlags.Spellcheck) };
```

### Enabling and Disabling Spell Checking

The [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) property controls whether spell checking is enabled. By default, the property is set to `true`. As the user enters text, misspellings are indicated.

However, for some text entry scenarios, such as entering a username, spell checking provides a negative experience and so should be disabled by setting the [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) property to `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> When the [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) property is set to `false`, and a custom keyboard isn't being used, the native spell checker will be disabled. However, if a [`Keyboard`](xref:Xamarin.Forms.Keyboard) has been set that disables spell checking, such as [`Keyboard.Chat`](xref:Xamarin.Forms.Keyboard.Chat), the `IsSpellCheckEnabled` property is ignored. Therefore, the property cannot be used to enable spell checking for a `Keyboard` that explicitly disables it.

### Placeholders

`Entry` can be set to show placeholder text when it is not storing user input. In practice, this is often seen in forms to clarify the content that is appropriate for a given field. Placeholder text color cannot be customized and will be the same regardless of the `TextColor` setting. If your design calls for a custom placeholder color, you'll need to fall back to a [custom renderer](). The following will create an `Entry` with "Username" as the placeholder in XAML:

```xaml
<Entry Placeholder="Username" />
```

In C#:

```csharp
var MyEntry = new Entry { Placeholder = "Username" };
```

![](entry-images/placeholder.png "Entry Placeholder Example")

### Password Fields

`Entry` provides the `IsPassword` property. When `IsPassword` is `true`, the contents of the field will be presented as black circles:

In XAML:

```xaml
<Entry IsPassword="true" />
```

In C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![](entry-images/password.png "Entry IsPassword Example")

Placeholders may be used with instances of `Entry` that are configured as password fields:

In XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

In C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![](entry-images/passwordplaceholder.png "Entry IsPassword and Placeholder Example")


### Colors

Entry can be set to use a custom background and text colors via the following bindable properties:

- **TextColor** &ndash; sets the color of the text.
- **BackgroundColor** &ndash; sets the color shown behind the text.

Special care is necessary to ensure that colors will be usable on each platform. Because each platform has different defaults for text and background colors, you'll often need to set both if you set one.

Use the following code to set the text color of an entry:

In XAML:

```xaml
<Entry TextColor="Green" />
```

In C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![](entry-images/textcolor.png "Entry TextColor Example")

Note that the placeholder is not affected by the specified `TextColor`.

To set the background color in XAML:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

In C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![](entry-images/textbackgroundcolor.png "Entry BackgroundColor Example")

Be careful to make sure that the background and text colors you choose are usable on each platform and don't obscure any placeholder text.

## Events and Interactivity

Entry exposes two events:

- [TextChanged](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/) &ndash; raised when the text changes in the entry. Provides the text before and after the change.
- [Completed](http://developer.xamarin.com/api/event/Xamarin.Forms.Entry.Completed/) &ndash; raised when the user has ended input by pressing the return key on the keyboard.

### Completed

The `Completed` event is used to react to the completion of an interaction with an Entry. `Completed` is raised when the user ends input with a field by entering the return key on the keyboard. The handler for the event is a generic event handler, taking the sender and `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

The completed event can be subscribed to in XAML:

```xaml
<Entry Completed="Entry_Completed" />
```

and C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

### TextChanged

The `TextChanged` event is used to react to a change in the content of a field.

`TextChanged` is raised whenever the `Text` of the `Entry` changes. The handler for the event takes an instance of `TextChangedEventArgs`. `TextChangedEventArgs` provides access to the old and new values of the `Entry` `Text` via the `OldTextValue` and `NewTextValue` properties:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

The `TextChanged` event can be subscribed to in XAML:

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

and C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```


## Related Links

- [Text (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Entry API](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)
