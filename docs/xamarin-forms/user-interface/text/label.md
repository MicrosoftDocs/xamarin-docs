---
title: "Xamarin.Forms Label"
description: "This article explains how to use the Xamarin.Forms Label class to display single and multi-line text in applications."
ms.prod: xamarin
ms.assetid: 02E6C553-5670-49A0-8EE9-5153ED21EA91
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/16/2018
---

# Xamarin.Forms Label

_Display text in Xamarin.Forms_

The [`Label`](xref:Xamarin.Forms.Label) view is used for displaying text, both single and multi-line. Labels can have custom fonts (families, sizes, and options) and colored text.

<a name="Truncation_and_Wrapping" />

## Truncation and Wrapping

Labels can be set to handle text that can't fit on one line in one of several ways, exposed by the `LineBreakMode` property. [`LineBreakMode`](xref:Xamarin.Forms.LineBreakMode) is an enumeration with the following values:

- **HeadTruncation** &ndash; truncates the head of the text, showing the end.
- **CharacterWrap** &ndash; wraps text onto a new line at a character boundary.
- **MiddleTruncation** &ndash; displays the beginning and end of the text, with the middle replace by an ellipsis.
- **NoWrap** &ndash; does not wrap text, displaying only as much text as can fit on one line.
- **TailTruncation** &ndash; shows the beginning of the text, truncating the end.
- **WordWrap** &ndash; wraps text at the word boundary.

## Fonts

For more information about specifying fonts on a `Label`, see [Fonts](~/xamarin-forms/user-interface/text/fonts.md).

## Colors

`Label`s can be set to use a custom text color via the bindable [`TextColor`](xref:Xamarin.Forms.Label.TextColor) property.

Special care is necessary to ensure that colors will be usable on each platform. Because each platform has different defaults for text and background colors, you'll need to be careful to pick a default that works on each.

Use the following XAML to set the text color of a label:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo">
    <StackLayout Padding="5,10">
      <Label TextColor="#77d065" FontSize = "20" Text="This is a label." />
    </StackLayout>
</ContentPage>
```

The equivalent C# code is:

```csharp
public partial class LabelPage : ContentPage
{
    public LabelPage ()
    {
        InitializeComponent ();

        var layout = new StackLayout { Padding = new Thickness(5,10) };
        var label = new Label { Text="This is a label.", TextColor = Color.FromHex("#77d065"), FontSize = 20 };
        layout.Children.Add(label);
        this.Content = layout;
    }
}
```

The following screenshots show the result of setting the `TextColor` property:

![](label-images/textcolor.png "Label TextColor Example")

For more information about colors, see [Colors](~/xamarin-forms/user-interface/colors.md).

<a name="Formatted_Text" />

## Formatted Text

Labels expose a [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) property which allows the presentation of text with multiple fonts and colors in the same view.

The `FormattedText` property is of type [`FormattedString`](xref:Xamarin.Forms.FormattedString), which comprises one or more [`Span`](xref:Xamarin.Forms.Span) instances, set via the [`Spans`](xref:Xamarin.Forms.FormattedString.Spans) property. Each `Span` possesses the following properties:

- [`BackgroundColor`](xref:Xamarin.Forms.Span.BackgroundColor) – the color of the span background.
- [`Font`](xref:Xamarin.Forms.Span.Font) – the font for the text in the span.
- [`FontAttributes`](xref:Xamarin.Forms.Span.FontAttributes) – the font attributes for the text in the span.
- [`FontFamily`](xref:Xamarin.Forms.Span.FontFamily) – the font family to which the font for the text in the span belongs.
- [`FontSize`](xref:Xamarin.Forms.Span.FontSize) – the size of the font for the text in the span.
- [`ForegroundColor`](xref:Xamarin.Forms.Span.ForegroundColor) – the color for the text in the span. This property is obsolete and has been replaced by the `TextColor` property.
- [`Style`](xref:Xamarin.Forms.Span.Style) – the style to apply to the span.
- [`Text`](xref:Xamarin.Forms.Span.Text) – the text of the span.
- [`TextColor`](xref:Xamarin.Forms.Span.TextColor) – the color for the text in the span.

The following XAML example demonstrates a `FormattedText` property that consists of three `Span` instances:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="TextSample.LabelPage"
             Title="Label Demo - XAML">
    <StackLayout Padding="5,10">
        ...
        <Label>
            <Label.FormattedText>
                <FormattedString>
                    <Span Text="Red Bold, " TextColor="Red" FontAttributes="Bold" />
                    <Span Text="default, " Style="{DynamicResource BodyStyle}" />
                    <Span Text="italic small." FontAttributes="Italic" FontSize="Small" />
                </FormattedString>
            </Label.FormattedText>
        </Label>
    </StackLayout>
</ContentPage>
```

The equivalent C# code is:

```csharp
public class LabelPageCode : ContentPage
{
    public LabelPageCode ()
    {
        var layout = new StackLayout{ Padding = new Thickness (5, 10) };
        ...
        var formattedString = new FormattedString ();
        formattedString.Spans.Add (new Span{ Text = "Red bold, ", ForegroundColor = Color.Red, FontAttributes = FontAttributes.Bold });
        formattedString.Spans.Add (new Span { Text = "default, ", Style = Device.Styles.BodyStyle });
        formattedString.Spans.Add (new Span { Text = "italic small.", FontAttributes = FontAttributes.Italic, FontSize =  Device.GetNamedSize(NamedSize.Small, typeof(Label)) });

        layout.Children.Add (new Label { FormattedText = formattedString });
        this.Content = layout;
    }
}
```

> [!NOTE]
> The [`Text`](xref:Xamarin.Forms.Span.Text) property of a `Span` can be set through data binding. For more information, see [Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md).

The following screenshots show the result of setting the `FormattedString` property to three `Span` instances:

![](label-images/formattedtext.png "Label FormattedText Example")

## Styling a Label

The previous sections covered setting [`Label`](xref:Xamarin.Forms.Label) properties on a per-instance basis. However, sets of properties can be grouped into one style that is consistently applied to one or many views. This can increase readability of code and make design changes easier to implement. For more information, see [Styles](~/xamarin-forms/user-interface/text/styles.md).

## Related Links

- [Creating Mobile Apps with Xamarin.Forms, Chapter 3](https://developer.xamarin.com/r/xamarin-forms/book/chapter03.pdf)
- [Text (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Label API](xref:Xamarin.Forms.Label)
