---
title: "Add a pan gesture recognizer"
description: "This article explains how to use a pan gesture to horizontally and vertically pan an image, so that all of the image content can be viewed when it's being displayed in a viewport smaller than the image dimensions."
ms.service: xamarin
ms.assetid: 42CBD2CF-432D-4F19-A05E-D569BB7F8713
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Add a pan gesture recognizer

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/workingwithgestures-pangesture)

_The pan gesture is used for detecting the movement of fingers around the screen and applying that movement to content, and is implemented with the `PanGestureRecognizer` class. A common scenario for the pan gesture is to horizontally and vertically pan an image, so that all of the image content can be viewed when it's being displayed in a viewport smaller than the image dimensions. This is accomplished by moving the image within the viewport, and is demonstrated in this article._

To make a user interface element moveable with the pan gesture, create a [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) instance, handle the [`PanUpdated`](xref:Xamarin.Forms.PanGestureRecognizer.PanUpdated) event, and add the new gesture recognizer to the [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) collection on the user interface element. The following code example shows a `PanGestureRecognizer` attached to an [`Image`](xref:Xamarin.Forms.Image) element:

```csharp
var panGesture = new PanGestureRecognizer();
panGesture.PanUpdated += (s, e) => {
  // Handle the pan
};
image.GestureRecognizers.Add(panGesture);
```

This can also be achieved in XAML, as shown in the following code example:

```xaml
<Image Source="MonoMonkey.jpg">
  <Image.GestureRecognizers>
    <PanGestureRecognizer PanUpdated="OnPanUpdated" />
  </Image.GestureRecognizers>
</Image>
```

The code for the `OnPanUpdated` event handler is then added to the code-behind file:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  // Handle the pan
}
```

## Creating a pan container

This section contains a generalized helper class that performs freeform panning, which is typically suited to navigating within images or maps. Handling the pan gesture to perform this operation requires some math to transform the user interface. This math is used to pan only within the bounds of the wrapped user interface element. The following code example shows the `PanContainer` class:

```csharp
public class PanContainer : ContentView
{
  double x, y;

  public PanContainer ()
  {
    // Set PanGestureRecognizer.TouchPoints to control the
    // number of touch points needed to pan
    var panGesture = new PanGestureRecognizer ();
    panGesture.PanUpdated += OnPanUpdated;
    GestureRecognizers.Add (panGesture);
  }

  void OnPanUpdated (object sender, PanUpdatedEventArgs e)
  {
    ...
  }
}
```

This class can be wrapped around a user interface element so that the gesture will pan the wrapped user interface element. The following XAML code example shows the `PanContainer` wrapping an [`Image`](xref:Xamarin.Forms.Image) element:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PanGesture"
             x:Class="PanGesture.HomePage">
    <ContentPage.Content>
        <AbsoluteLayout>
            <local:PanContainer>
                <Image Source="MonoMonkey.jpg" WidthRequest="1024" HeightRequest="768" />
            </local:PanContainer>
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

The following code example shows how the `PanContainer` wraps an [`Image`](xref:Xamarin.Forms.Image) element in a C# page:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new AbsoluteLayout {
      Padding = new Thickness (20),
      Children = {
        new PanContainer {
          Content = new Image {
            Source = ImageSource.FromFile ("MonoMonkey.jpg"),
            WidthRequest = 1024,
            HeightRequest = 768
          }
        }
      }
    };
  }
}
```

In both examples, the [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) and [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) properties are set to the width and height values of the image being displayed.

When the [`Image`](xref:Xamarin.Forms.Image) element receives a pan gesture, the displayed image will be panned. The pan is performed by the `PanContainer.OnPanUpdated` method, which is shown in the following code example:

```csharp
void OnPanUpdated (object sender, PanUpdatedEventArgs e)
{
  switch (e.StatusType) {
  case GestureStatus.Running:
    // Translate and ensure we don't pan beyond the wrapped user interface element bounds.
    Content.TranslationX =
      Math.Max (Math.Min (0, x + e.TotalX), -Math.Abs (Content.Width - App.ScreenWidth));
    Content.TranslationY =
      Math.Max (Math.Min (0, y + e.TotalY), -Math.Abs (Content.Height - App.ScreenHeight));
    break;

  case GestureStatus.Completed:
    // Store the translation applied during the pan
    x = Content.TranslationX;
    y = Content.TranslationY;
    break;
  }
}
```

This method updates the viewable content of the wrapped user interface element, based on the user's pan gesture. This is achieved by using the values of the [`TotalX`](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalX) and [`TotalY`](xref:Xamarin.Forms.PanUpdatedEventArgs.TotalY) properties of the [`PanUpdatedEventArgs`](xref:Xamarin.Forms.PanUpdatedEventArgs) instance to calculate the direction and distance of the pan. The `App.ScreenWidth` and `App.ScreenHeight` properties provide the height and width of the viewport, and are set to the screen width and screen height values of the device by the respective platform-specific projects. The wrapped user element is then panned by setting its [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) and [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) properties to the calculated values.

When panning content in an element that does not occupy the full screen, the height and width of the viewport can be obtained from the element's [`Height`](xref:Xamarin.Forms.VisualElement.Height) and [`Width`](xref:Xamarin.Forms.VisualElement.Width) properties.

> [!NOTE]
> Displaying high-resolution images can greatly increase an app's memory footprint. Therefore, they should only be created when required and should be released as soon as the app no longer requires them. For more information, see [Optimize Image Resources](~/xamarin-forms/deploy-test/performance.md#optimize-image-resources).

## Related Links

- [PanGesture (sample)](/samples/xamarin/xamarin-forms-samples/workingwithgestures-pangesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PanGestureRecognizer](xref:Xamarin.Forms.PanGestureRecognizer)