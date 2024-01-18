---
title: "Xamarin.Forms ImageButton"
description: "The ImageButton displays an image and responds to a tap or click that directs an application to carry out a particular task."
ms.service: xamarin
ms.assetid: B5906AB6-3F79-4FCB-8C78-1F0AF18AB39E
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/04/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms ImageButton

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/formsgallery)

_The ImageButton displays an image and responds to a tap or click that directs an application to carry out a particular task._

The `ImageButton` view combines the [`Button`](xref:Xamarin.Forms.Button) view and [`Image`](xref:Xamarin.Forms.Image) view to create a button whose content is an image. The user presses the `ImageButton` with a finger or clicks it with a mouse to direct the application to carry out a particular task. However, unlike the `Button` view, the `ImageButton` view has no concept of text and text appearance.

> [!NOTE]
> While the [`Button`](xref:Xamarin.Forms.Button) view defines an [`Image`](xref:Xamarin.Forms.Button.Image) property, that allows you to display a image on the `Button`, this property is intended to be used when displaying a small icon next to the `Button` text.

The code examples in this guide are taken from the [FormsGallery sample](/samples/xamarin/xamarin-forms-samples/formsgallery).

## Setting the image source

`ImageButton` defines a `Source` property that should be set to the image to display in the button, with the image source being either a file, a URI, a resource, or a stream. For more information about loading images from different sources, see [Images in Xamarin.Forms](images.md).

The following example shows how to instantiate a `ImageButton` in XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

The `Source` property specifies the image that appears in the `ImageButton`. In this example it's set to a local file that will be loaded from each platform project, resulting in the following screenshots:

[![Basic ImageButton](imagebutton-images/BasicImageButton.png "Basic ImageButton")](imagebutton-images/BasicImageButton-Large.png#lightbox "Basic ImageButton")

By default, the `ImageButton` is rectangular, but you can give it rounded corners by using the `CornerRadius` property. For more information about `ImageButton` appearance, see [ImageButton appearance](#imagebutton-appearance).

> [!NOTE]
> While an `ImageButton` can load an animated GIF, it will only display the first frame of the GIF.

The following example shows how to create a page that is functionally equivalent to the previous XAML example, but entirely in C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children = { header, imageButton }
        };
    }
}
```

## Handling ImageButton clicks

`ImageButton` defines a `Clicked` event that is fired when the user taps the `ImageButton` with a finger or mouse pointer. The event is fired when the finger or mouse button is released from the surface of the `ImageButton`. The `ImageButton` must have its `IsEnabled` property set to `true` to respond to taps.

The following example shows how to instantiate a `ImageButton` in XAML and handle its `Clicked` event:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="FormsGallery.XamlExamples.ImageButtonDemoPage"
             Title="ImageButton Demo">
    <StackLayout>
        <Label Text="ImageButton"
               FontSize="50"
               FontAttributes="Bold"
               HorizontalOptions="Center" />

       <ImageButton Source="XamarinLogo.png"
                    HorizontalOptions="Center"
                    VerticalOptions="CenterAndExpand"
                    Clicked="OnImageButtonClicked" />

        <Label x:Name="label"
               Text="0 ImageButton clicks"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

The `Clicked` event is set to an event handler named `OnImageButtonClicked` that is located in the code-behind file:

```csharp
public partial class ImageButtonDemoPage : ContentPage
{
    int clickTotal;

    public ImageButtonDemoPage()
    {
        InitializeComponent();
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

When the `ImageButton` is tapped, the `OnImageButtonClicked` method executes. The `sender` argument is the `ImageButton` responsible for this event. You can use this to access the `ImageButton` object, or to distinguish between multiple `ImageButton` objects sharing the same `Clicked` event.

This particular `Clicked` handler increments a counter and displays the counter value in a [`Label`](xref:Xamarin.Forms.Label):

[![Basic ImageButton Click](imagebutton-images/ImageButton.png "Basic ImageButton Click")](imagebutton-images/ImageButton-Large.png#lightbox "Basic ImageButton Click")

The following example shows how to create a page that is functionally equivalent to the previous XAML example, but entirely in C#:

```csharp
public class ImageButtonDemoPage : ContentPage
{
    Label label;
    int clickTotal = 0;

    public ImageButtonDemoPage()
    {
        Label header = new Label
        {
            Text = "ImageButton",
            FontSize = 50,
            FontAttributes = FontAttributes.Bold,
            HorizontalOptions = LayoutOptions.Center
        };

        ImageButton imageButton = new ImageButton
        {
            Source = "XamarinLogo.png",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };
        imageButton.Clicked += OnImageButtonClicked;

        label = new Label
        {
            Text = "0 ImageButton clicks",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        // Build the page.
        Title = "ImageButton Demo";
        Content = new StackLayout
        {
            Children =
            {
                header,
                imageButton,
                label
            }
        };
    }

    void OnImageButtonClicked(object sender, EventArgs e)
    {
        clickTotal += 1;
        label.Text = $"{clickTotal} ImageButton click{(clickTotal == 1 ? "" : "s")}";
    }
}
```

## Disabling the ImageButton

Sometimes an application is in a particular state where a particular `ImageButton` click is not a valid operation. In those cases, the `ImageButton` should be disabled by setting its `IsEnabled` property to `false`.

## Using the command interface

It is possible for an application to respond to `ImageButton` taps without handling the `Clicked` event. The `ImageButton` implements an alternative notification mechanism called the _command_ or _commanding_ interface. This consists of two properties:

- `Command` of type [`ICommand`](xref:System.Windows.Input.ICommand), an interface defined in the [`System.Windows.Input`](xref:System.Windows.Input) namespace.
- `CommandParameter` property of type [`Object`](xref:System.Object).

This approach is suitable in connection with data-binding, and particularly when implementing the Model-View-ViewModel (MVVM) architecture.

For more information about using the command interface, see [Using the command interface](button.md#using-the-command-interface) in the [Button](button.md) guide.

## Pressing and releasing the ImageButton

Besides the `Clicked` event, `ImageButton` also defines `Pressed` and `Released` events. The `Pressed` event occurs when a finger presses on a `ImageButton`, or a mouse button is pressed with the pointer positioned over the `ImageButton`. The `Released` event occurs when the finger or mouse button is released. Generally, the `Clicked` event is also fired at the same time as the `Released` event, but if the finger or mouse pointer slides away from the surface of the `ImageButton` before being released, the `Clicked` event might not occur.

For more information about these events, see [Pressing and releasing the button](button.md#pressing-and-releasing-the-button) in the [Button](button.md) guide.

## ImageButton appearance

In addition to the properties that `ImageButton` inherits from the [`View`](xref:Xamarin.Forms.View) class, `ImageButton` also defines several properties that affect its appearance:

- `Aspect` is how the image will be scaled to fit the display area.
- `BorderColor` is the color of an area surrounding the `ImageButton`.
- `BorderWidth` is the width of the border.
- `CornerRadius` is the corner radius of the `ImageButton`.

The `Aspect` property can be set to one of the members of the [`Aspect`](xref:Xamarin.Forms.Aspect) enumeration:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) - stretches the image to completely and exactly fill the `ImageButton`. This may result in the image being distorted.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) - clips the image so that it fills the `ImageButton` while preserving the aspect ratio.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) - letterboxes the image (if necessary) so that the entire image fits into the `ImageButton`, with blank space added to the top/bottom or sides depending on whether the image is wide or tall. This is the default value of the [`Aspect`](xref:Xamarin.Forms.Aspect) enumeration.

> [!NOTE]
> The `ImageButton` class also has [`Margin`](xref:Xamarin.Forms.View.Margin) and `Padding` properties that control the layout behavior of the `ImageButton`. For more information, see [Margin and Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

## ImageButton visual states

`ImageButton` has a `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) that can be used to initiate a visual change to the `ImageButton` when pressed by the user, provided that it's enabled.

The following XAML example shows how to define a visual state for the `Pressed` state:

```xaml
<ImageButton Source="XamarinLogo.png"
             ...>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="1" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Pressed">
                <VisualState.Setters>
                    <Setter Property="Scale"
                            Value="0.8" />
                </VisualState.Setters>
            </VisualState>

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</ImageButton>
```

The `Pressed` [`VisualState`](xref:Xamarin.Forms.VisualState) specifies that when the `ImageButton` is pressed, its [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) property will be changed from its default value of 1 to 0.8. The `Normal` `VisualState` specifies that when the `ImageButton` is in a normal state, its `Scale` property will be set to 1. Therefore, the overall effect is that when the `ImageButton` is pressed, it's rescaled to be slightly smaller, and when the `ImageButton` is released, it's rescaled to its default size.

For more information about visual states, see [The Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## Related links

- [FormsGallery sample](/samples/xamarin/xamarin-forms-samples/formsgallery)