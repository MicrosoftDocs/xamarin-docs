---
title: "Label"
description: "Display text in Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
---

# Label

_Display text in Xamarin.Forms_

The `Label` view is used for displaying text, both single and multi-line. Labels can have custom fonts (families, sizes, and options) and colored text. This article covers the following topics:

- **[Truncation and Wrapping](#Truncation_and_Wrapping)** &ndash; truncation and wrapping options for handling situations where text can't fit on one line.
- **[Font](#Font)** &ndash; font options.
- **[Color](#Color)** &ndash; label text color options.
- **[Formatted Text](#Formatted_Text)** &ndash; options for displaying text with multiple formats/styles inline.

## Styling Label

The following sections cover setting properties of `Label` manually on a per-instance basis. Note that sets of properties can be grouped into one style that is consistently applied to one or many views. This can increase readability of code and make design changes easier to implement. See [Styles](~/xamarin-forms/user-interface/text/styles.md) for more information.

<a name="Truncation_and_Wrapping" />

## Truncation and Wrapping

Labels can be set to handle text that can't fit on one line in one of several ways, exposed by the `LineBreakMode` property. [`LineBreakMode`](https://developer.xamarin.com/api/type/Xamarin.Forms.LineBreakMode/) is an enumeration of the following options:

- **HeadTruncation** &ndash; truncates the head of the text, showing the end.
- **CharacterWrap** &ndash; wraps text onto a new line at a character boundary.
- **MiddleTruncation** &ndash; displays the beginning and end of the text, with the middle replace by an ellipsis.
- **NoWrap** &ndash; does not wrap text, displaying only as much text as can fit on one line.
- **TailTruncation** &ndash; shows the beginning of the text, truncating the end.
- **WordWrap** &ndash; wraps text at the word boundary.

## Font

See [Working with Fonts](~/xamarin-forms/user-interface/text/fonts.md) for more information.

## Color

`Label`s can be set to use a custom text color via the bindable `TextColor` property.

Special care is necessary to ensure that colors will be usable on each platform. Because each platform has different defaults for text and background colors, you'll need to be careful to pick a default that works on each.

Use the following code to set the text color of a label:

In code:

```csharp
public partial class LabelPage : ContentPage
{
	public LabelPage ()
	{
		InitializeComponent ();
		var layout = new StackLayout { Padding = new Thickness(5,10) };
		this.Content = layout;
		var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
		layout.Children.Add(label);
	}
}
```

In XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
	<ContentPage.Content>
		<StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/textcolor.png "Label TextColor Example")

<a name="Formatted_Text" />

## Formatted Text

Labels expose a `FormattedText` property which allows you to present text with multiple fonts and colors in the same view.

The `FormattedText` property is of type [`FormattedString`](https://developer.xamarin.com/api/type/Xamarin.Forms.FormattedString/). Formatted Strings are composed of one or more `Span`s, each one with the following properties:

- **BackgroundColor** &ndash; can be used to set a background color, for example to achieve a highlighter effect.
- **FontAttributes** &ndash; can be set to bold, italic, or neither.
- **FontFamily** &ndash; sets the font to be used.
- **FontSize** &ndash; sets the size of the text.
- **ForegroundColor** &ndash; sets the color of the text.
- **Text** &ndash; the text to be presented.

The following C# code demonstrates a label where the first word is bold and the last word is red:

```csharp
public partial class LabelPage : ContentPage
{
	public LabelPage ()
	{
		InitializeComponent ();
		var layout = new StackLayout { Padding = new Thickness(5,10) };
		this.Content = layout;
    var label = new Label { FontSize = 20 };
    var s = new FormattedString ();
    s.Spans.Add (new Span{ Text = "Red Bold", FontAttributes = FontAttributes.Bold });
    s.Spans.Add (new Span{ Text = "Default" });
    s.Spans.Add (new Span{ Text = "italic small", FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)), FontAttributes = FontAttributes.Italic});
    label.FormattedText = s;
		layout.Children.Add(label);
	}
}
```

In XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.LabelPage"
Title="Label Demo">
	<ContentPage.Content>
		<StackLayout Padding="5,10">
      <Label FontSize=20>
        <Label.FormattedText>
          <FormattedString>
            <Span Text="Red Bold" ForegroundColor="Red" FontAttributes="Bold" />
            <Span Text="Default" />
            <Span Text="italic small" FontAttributes="Italic" FontSize="Small" />
          </FormattedString>
        </Label.FormattedText>
      </Label>
    </StackLayout>
  </ContentPage.Content>
</ContentPage>
```

![](label-images/formattedtext.png "Label FormattedText Example")


## Related Links

- [Creating Mobile Apps with Xamarin.Forms, Chapter 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Text (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Label API](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)
