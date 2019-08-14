---
title: "Fonts in Xamarin.Forms"
description: "This article explains how to specify font information on controls that display text in Xamarin.Forms applications."
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
---

# Fonts in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

This article describes how Xamarin.Forms lets you specify font attributes (including weight and size) on controls that display text. Font information can be [specified in code](#Setting_Font_in_Code) or [specified in XAML](#Setting_Font_in_Xaml). It's' also possible to use a [custom font](#Using_a_Custom_Font), and [display font icons](#display-font-icons).

<a name="Setting_Font_in_Code" />

## Set the font in code

Use the three font-related properties of any controls that display text:

- **FontFamily** &ndash; the `string` font name.
- **FontSize** &ndash; the font size as a `double`.
- **FontAttributes** &ndash; a string specifying style information like *Italic* and **Bold** (using the `FontAttributes` enumeration in C#).

This code shows how to create a label and specify the font size and weight to display:

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### Font size

The `FontSize` property can be set to a double value, for instance:

```csharp
label.FontSize = 24;
```

Xamarin.Forms also defines fields in the [`NamedSize`](xref:Xamarin.Forms.NamedSize) enumeration that represent specific font sizes. For more information about named font sizes, see [Named font sizes](#named-font-sizes).

<a name="FontAttributes" />

### Font attributes

Font styles such as **bold** and *italic* can be set on the `FontAttributes` property. The following values are currently supported:

-  **None**
-  **Bold**
-  **Italic**

The `FontAttribute` enumeration can be used as follows (you can specify a single attribute or `OR` them together):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### Set font info per platform

Alternatively, the `Device.RuntimePlatform` property can be used to set different
font names on each platform, as demonstrated in this code:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

A good source of font information for iOS is [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

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

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values) can also be used in XAML to render a different font on each platform. The example below uses a custom font face on iOS (MarkerFelt-Thin) and specifies only size/attributes on the other platforms:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

When specifying a custom font face, it is always a good idea to use `OnPlatform`, as it is difficult to find a font that is available on all platforms.

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

Named font sizes can be set through both XAML and code. In addition, the `Device.GetNamedSize` method can be called to return a `double` that represents the named font size:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> On iOS and Android, named font sizes will autoscale based on operating system accessibility options. This behavior can be disabled on iOS with a platform-specific. For more information, see [Accessibility Scaling for Named Font Sizes on iOS](~/xamarin-forms/platform/ios/named-font-size-scaling.md).

<a name="Using_a_Custom_Font" />

## Use a custom font

Using a font other than the built-in typefaces requires some platform-specific coding. This screenshot shows the custom font **Lobster** from [Google's open-source fonts](https://www.google.com/fonts) rendered using Xamarin.Forms.

 [![Custom font on iOS and Android](fonts-images/custom-sml.png "Custom Fonts Example")](fonts-images/custom.png#lightbox "Custom Fonts Example")

The steps required for each platform are outlined below. When including custom font files with an application, be sure to verify that the font's license allows for distribution.

### iOS

It is possible to display a custom font by first ensuring that it is loaded,
then referring to it by name using the Xamarin.Forms `Font` methods.
Follow the instructions in [this blog post](https://blog.xamarin.com/custom-fonts-in-ios/):

1. Add the font file with **Build Action: BundleResource**, and
2. Update the **Info.plist** file (**Fonts provided by application**, or `UIAppFonts`, key), then
3. Refer to it by name wherever you define a font in Xamarin.Forms!

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### Android

Xamarin.Forms for Android can reference a custom font that has been added to the project by following a specific naming standard. First add the font file to the **Assets** folder in the application project and set *Build Action: AndroidAsset*. Then use the full path and *Font Name* separated by a hash (#) as the font name in Xamarin.Forms, as the code snippet below demonstrates:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### Windows

Xamarin.Forms for Windows platforms can reference a custom font that has been added to the project by following a specific naming standard. First add the font file to the **/Assets/Fonts/** folder in the application project and set the **Build Action:Content**. Then use the full path and font filename, followed by a hash (#) and the **Font Name**, as the code snippet below demonstrates:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Note that the font file name and font name may be different. To discover the font name on Windows, right-click the .ttf file and select **Preview**. The font name can then be determined from the preview window.

The common code for the application is now complete. Platform-specific phone dialer code will now be implemented as a [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

### XAML

You can also use [`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads) in XAML to render a custom font:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

## Display font icons

Font icons can be displayed by Xamarin.Forms applications by specifying the font icon data in a `FontImageSource` object. This class, which derives from the [`ImageSource`](xref:Xamarin.Forms.ImageSource) class, has the following properties:

- `Glyph` – the unicode character value of the font icon, specified as a `string`.
- `Size` – a `double` value that indicates the size, in device-independent units, of the rendered font icon. The default value is 30.
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

The following screenshots, from the [Bindable Layouts](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) sample, show several font icons being displayed by a bindable layout:

![Screenshot of font icons being displayed, on iOS and Android](fonts-images/font-image-source.png "Font icons being displayed in an Image view")

## Related links

- [FontsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Text (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Bindable Layouts (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Bindable Layouts](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
