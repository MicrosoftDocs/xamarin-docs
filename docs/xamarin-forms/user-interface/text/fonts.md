---
title: "Fonts in Xamarin.Forms"
description: "This article explains how to specify font information on controls that display text in Xamarin.Forms applications."
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/01/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Fonts in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

This article describes how Xamarin.Forms lets you specify font attributes (including weight and size) on controls that display text. Font information can be [specified in code](#set-the-font-in-code) or [specified in XAML](#set-the-font-in-xaml). It's' also possible to use a [custom font](#use-a-custom-font), and [display font icons](#display-font-icons).

## Set the font in code

Use the three font-related properties of any controls that display text:

- **FontFamily** &ndash; the `string` font name.
- **FontSize** &ndash; the font size as a `double`.
- **FontAttributes** &ndash; a string specifying style information like *Italic* and **Bold** (using the `FontAttributes` enumeration in C#).

This code shows how to create a label and specify the font size and weight to display:

```csharp
var about = new Label
{
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

### Font size

The `FontSize` property can be set to a double value, for instance:

```csharp
label.FontSize = 24;
```

The size value is measured in device-independent units. For more information, see [Units of Measurement](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Xamarin.Forms also defines fields in the [`NamedSize`](xref:Xamarin.Forms.NamedSize) enumeration that represent specific font sizes. For more information about named font sizes, see [Named font sizes](#named-font-sizes).

### Font attributes

Font styles such as **bold** and *italic* can be set on the `FontAttributes` property. The following values are currently supported:

- **None**
- **Bold**
- **Italic**

The `FontAttribute` enumeration can be used as follows (you can specify a single attribute or `OR` them together):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### Set font info per platform

Alternatively, the `Device.RuntimePlatform` property can be used to set different font names on each platform, as demonstrated in this code:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

A good source of font information for iOS is [iosfonts.com](http://iosfonts.com).

## Set the font in XAML

Xamarin.Forms controls that display text all have a `FontSize` property that can be set in XAML. The simplest way to set the font in XAML is to use the named size enumeration values, as shown in this example:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

There is a built-in converter for the `FontSize` property that allows all font settings to be expressed as a string value in XAML. In addition, the `FontAttributes` property can be used to specify font attributes:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

The [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#provide-platform-specific-values) property can also be used in XAML to render a different font on each platform. The example below uses different fonts on each platform:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/ArimaMadurai-Black.ttf#Arima Madurai" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

## Named font sizes

Xamarin.Forms defines fields in the [`NamedSize`](xref:Xamarin.Forms.NamedSize) enumeration that represent specific font sizes. The following table shows the `NamedSize` members, and their default sizes on iOS, Android, and the Universal Windows Platform (UWP):

| Member | iOS | Android | UWP |
| --- | --- | --- | --- |
| `Default` | 16 | 14 | 14 |
| `Micro` | 11 | 10 | 15.667 |
| `Small` | 13 | 14 | 18.667 |
| `Medium` | 16 | 17 | 22.667 |
| `Large` | 20 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 96 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

The size values are measured in device-independent units. For more information, see [Units of Measurement](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Named font sizes can be set through both XAML and code. In addition, the `Device.GetNamedSize` method can be called to return a `double` that represents the named font size:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> On iOS and Android, named font sizes will autoscale based on operating system accessibility options. This behavior can be disabled on iOS with a platform-specific. For more information, see [Accessibility Scaling for Named Font Sizes on iOS](~/xamarin-forms/platform/ios/named-font-size-scaling.md).

## Use a custom font

Custom fonts can be added to your Xamarin.Forms shared project and consumed by platform projects without any additional work. The process for accomplishing this is as follows:

1. Add the font to your Xamarin.Forms shared project as an embedded resource (**Build Action: EmbeddedResource**).
1. Register the font file with the assembly, in a file such as **AssemblyInfo.cs**, using the `ExportFont` attribute. An optional alias can also be specified.

> [!IMPORTANT]
> Embedded fonts requires the use of Xamarin.Forms 4.5.0.530 or higher.

The following example shows the Lobster-Regular font being registered with the assembly, along with an alias:

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> The font can reside in any folder in the shared project, without having to specify the folder name when registering the font with the assembly.

The font can then be consumed on each platform by referencing its name, without the file extension:

```xaml
<!-- Use font name -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster-Regular" />
```

Alternatively, it can be consumed on each platform by referencing its alias:

```xaml
<!-- Use font alias -->
<Label Text="Hello Xamarin.Forms"
       FontFamily="Lobster" />
```

The equivalent C# code is:

```csharp
// Use font name
Label label1 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster-Regular"
};

// Use font alias
Label label2 = new Label
{
    Text = "Hello Xamarin.Forms!",
    FontFamily = "Lobster"
};
```

The following screenshots show the custom font:

[![Custom font on iOS and Android](fonts-images/custom-sml.png "Custom Fonts Example")](fonts-images/custom.png#lightbox "Custom Fonts Example")

> [!IMPORTANT]
> On Windows, the font file name and font name may be different. To discover the font name on Windows, right-click the .ttf file and select **Preview**. The font name can then be determined from the preview window.

## Display font icons

Font icons can be displayed by Xamarin.Forms applications by specifying the font icon data in a `FontImageSource` object. This class, which derives from the [`ImageSource`](xref:Xamarin.Forms.ImageSource) class, has the following properties:

- `Glyph` – the unicode character value of the font icon, specified as a `string`.
- `Size` – a `double` value that indicates the size, in device-independent units, of the rendered font icon. The default value is 30. In addition, this property can be set to a named font size.
- `FontFamily` – a `string` representing the font family to which the font icon belongs.
- `Color` – an optional [`Color`](xref:Xamarin.Forms.Color) value to be used when displaying the font icon.

This data is used to create a PNG, which can be displayed by any view that can display an `ImageSource`. This approach permits font icons, such as emojis, to be displayed by multiple views, as opposed to limiting font icon display to a single text presenting view, such as a [`Label`](xref:Xamarin.Forms.Label).

> [!IMPORTANT]
> Font icons can only currently be specified by their unicode character representation.

The following XAML example has a single font icon being displayed by an [`Image`](xref:Xamarin.Forms.Image) view:

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

This code displays an XBox icon, from the Ionicons font family, in an [`Image`](xref:Xamarin.Forms.Image) view. Note that while the unicode character for this icon is `\uf30c`, it has to be escaped in XAML and so becomes `&#xf30c;`. The equivalent C# code is:

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

The following screenshots, from the [Bindable Layouts](/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) sample, show several font icons being displayed by a bindable layout:

![Screenshot of font icons being displayed, on iOS and Android](fonts-images/font-image-source.png "Font icons being displayed in an Image view")

## Related links

- [FontsSample](/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Text (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Bindable Layouts (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Bindable Layouts](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)