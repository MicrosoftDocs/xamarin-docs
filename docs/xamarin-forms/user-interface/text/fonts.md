---
title: "Fonts in Xamarin.Forms"
description: "This article explains how to specify font information on controls that display text in Xamarin.Forms applications."
ms.service: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Fonts in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/workingwithfonts)

By default, Xamarin.Forms uses a system font defined by each platform. However, controls that display text define properties that you can use to change this font:

- `FontAttributes`, of type `FontAttributes`, which is an enumeration with three members: `None`, `Bold`, and `Italic`. The default value of this property is `None`.
- `FontSize`, of type `double`.
- `FontFamily`, of type `string`.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that they can be targets of data bindings, and styled.

## Set font attributes

Controls that display text can set the `FontAttributes` property to specify font attributes:

```xaml
<Label Text="Italics"
       FontAttributes="Italic" />
<Label Text="Bold and italics"
       FontAttributes="Bold, Italic" />
```

The equivalent C# code is:

```csharp
Label label1 = new Label
{
    Text = "Italics",
    FontAttributes = FontAttributes.Italic
};

Label label2 = new Label
{
    Text = "Bold and italics",
    FontAttributes = FontAttributes.Bold | FontAttributes.Italic
};    
```

## Set the font size

Controls that display text can set the `FontSize` property to specify the font size. The `FontSize` property can be set to a `double` value directly, or by a [`NamedSize`](xref:Xamarin.Forms.NamedSize) enumeration value:

```xaml
<Label Text="Font size 24"
       FontSize="24" />
<Label Text="Large font size"
       FontSize="Large" />
```

The equivalent C# code is:

```csharp
Label label1 = new Label
{
    Text = "Font size 24",
    FontSize = 24
};

Label label2 = new Label
{
    Text = "Large font size",
    FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label))
};
```

Alternatively, the `Device.GetNamedSize` method has an override that specifies the second argument as an [`Element`](xref:Xamarin.Forms.Element):

```csharp
Label myLabel = new Label
{
    Text = "Large font size",
};
myLabel.FontSize = Device.GetNamedSize(NamedSize.Large, myLabel);
```

> [!NOTE]
> The `FontSize` value, when specified as a `double`, is measured in device-independent units. For more information, see [Units of Measurement](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

For more information about named font sizes, see [Understand named font sizes](#understand-named-font-sizes).

## Set the font family

Controls that display text can set the `FontFamily` property to a font family name, such as "Times Roman". However, this will only work if that font family is supported on the particular platform.

There are a number of techniques that can be used to attempt to derive the fonts that are available on a platform. However, the presence of a TTF (True Type Format) font file does not necessarily imply a font family, and TTFs are often included that are not intended for use in applications. In addition, the fonts installed on a platform can change with platform version. Therefore, the most reliable approach for specifying a font family is to use a custom font.

Custom fonts can be added to your Xamarin.Forms shared project and consumed by platform projects without any additional work. The process for accomplishing this is as follows:

1. Add the font to your Xamarin.Forms shared project as an embedded resource (**Build Action: EmbeddedResource**).
1. Register the font file with the assembly, in a file such as **AssemblyInfo.cs**, using the `ExportFont` attribute. An optional alias can also be specified.

The following example shows the Lobster-Regular font being registered with the assembly, along with an alias:

```csharp
using Xamarin.Forms;

[assembly: ExportFont("Lobster-Regular.ttf", Alias = "Lobster")]
```

> [!NOTE]
> The font can reside in any folder in the shared project, without having to specify the folder name when registering the font with the assembly.
>
> On Windows, the font file name and font name may be different. To discover the font name on Windows, right-click the .ttf file and select **Preview**. The font name can then be determined from the preview window.

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
> For release builds on Windows, ensure the assembly containing the custom font is passed as an argument in the `Forms.Init` method call. For more information, see [Troubleshooting](~/xamarin-forms/platform/windows/installation/index.md#troubleshooting).

## Set font properties per platform

The [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) and [`On`](xref:Xamarin.Forms.On) classes can be used in XAML to set font properties per platform. The example below sets different font families and sizes on each platform:

```xaml
<Label Text="Different font properties on different platforms"
       FontSize="{OnPlatform iOS=20, Android=Medium, UWP=24}">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
            <On Platform="iOS" Value="MarkerFelt-Thin" />
            <On Platform="Android" Value="Lobster-Regular" />
            <On Platform="UWP" Value="ArimaMadurai-Black" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

The [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#provide-platform-specific-values) property can be used in code to set font properties per platform

```csharp
Label label = new Label
{
    Text = "Different font properties on different platforms"
};

label.FontSize = Device.RuntimePlatform == Device.iOS ? 20 :
    Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : 24;
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "MarkerFelt-Thin" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular" : "ArimaMadurai-Black";
```

For more information about providing platform-specific values, see [Provide platform-specific values](~/xamarin-forms/platform/device.md#provide-platform-specific-values). For information about the `OnPlatform` markup extension, see [OnPlatform markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## Understand named font sizes

Xamarin.Forms defines fields in the [`NamedSize`](xref:Xamarin.Forms.NamedSize) enumeration that represent specific font sizes. The following table shows the `NamedSize` members, and their default sizes on iOS, Android, and the Universal Windows Platform (UWP):

| Member | iOS | Android | UWP |
| --- | --- | --- | --- |
| `Default` | 17 | 14 | 14 |
| `Micro` | 12 | 10 | 15.667 |
| `Small` | 14 | 14 | 18.667 |
| `Medium` | 17 | 17 | 22.667 |
| `Large` | 22 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 14 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

The size values are measured in device-independent units. For more information, see [Units of Measurement](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

> [!NOTE]
> On iOS and Android, named font sizes will autoscale based on operating system accessibility options. This behavior can be disabled on iOS with a platform-specific. For more information, see [Accessibility Scaling for Named Font Sizes on iOS](~/xamarin-forms/platform/ios/named-font-size-scaling.md).

## Display font icons

Font icons can be displayed by Xamarin.Forms applications by specifying the font icon data in a `FontImageSource` object. This class, which derives from the [`ImageSource`](xref:Xamarin.Forms.ImageSource) class, has the following properties:

- `Glyph` – the unicode character value of the font icon, specified as a `string`.
- `Size` – a `double` value that indicates the size, in device-independent units, of the rendered font icon. The default value is 30. In addition, this property can be set to a named font size.
- `FontFamily` – a `string` representing the font family to which the font icon belongs.
- `Color` – an optional [`Color`](xref:Xamarin.Forms.Color) value to be used when displaying the font icon.

The font data can be displayed by any view that can display an `ImageSource`. This approach permits font icons, such as emojis, to be displayed by multiple views, as opposed to limiting font icon display to a single text presenting view, such as a [`Label`](xref:Xamarin.Forms.Label).

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
- [Provide platform-specific values](~/xamarin-forms/platform/device.md#provide-platform-specific-values)
- [OnPlatform markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension)
- [Bindable Layouts](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
