---
title: "Add a pinch gesture recognizer"
description: "This article explains how to use the pinch gesture to perform interactive zoom of an image at the pinch location."
ms.service: xamarin
ms.assetid: 832F7810-F0CF-441A-B04A-3975F3FB8B29
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Add a pinch gesture recognizer

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/workingwithgestures-pinchgesture)

_The pinch gesture is used for performing interactive zoom and is implemented with the PinchGestureRecognizer class. A common scenario for the pinch gesture is to perform interactive zoom of an image at the pinch location. This is accomplished by scaling the content of the viewport, and is demonstrated in this article._

To make a user interface element zoomable with the pinch gesture, create a [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer) instance, handle the [`PinchUpdated`](xref:Xamarin.Forms.PinchGestureRecognizer.PinchUpdated) event, and add the new gesture recognizer to the [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) collection on the user interface element. The following code example shows a `PinchGestureRecognizer` attached to an [`Image`](xref:Xamarin.Forms.Image) element:

```csharp
var pinchGesture = new PinchGestureRecognizer();
pinchGesture.PinchUpdated += (s, e) => {
  // Handle the pinch
};
image.GestureRecognizers.Add(pinchGesture);
```

This can also be achieved in XAML, as shown in the following code example:

```xaml
<Image Source="waterfront.jpg">
  <Image.GestureRecognizers>
    <PinchGestureRecognizer PinchUpdated="OnPinchUpdated" />
  </Image.GestureRecognizers>
</Image>
```

The code for the `OnPinchUpdated` event handler is then added to the code-behind file:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  // Handle the pinch
}
```

## Creating a PinchToZoom container

Handling the pinch gesture to perform a zoom operation requires some math to transform the user interface. This section contains a generalized helper class to perform the math, which can be used to interactively zoom any user interface element. The following code example shows the `PinchToZoomContainer` class:

```csharp
public class PinchToZoomContainer : ContentView
{
  ...

  public PinchToZoomContainer ()
  {
    var pinchGesture = new PinchGestureRecognizer ();
    pinchGesture.PinchUpdated += OnPinchUpdated;
    GestureRecognizers.Add (pinchGesture);
  }

  void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
  {
    ...
  }
}
```

This class can be wrapped around a user interface element so that the pinch gesture will zoom the wrapped user interface element. The following XAML code example shows the `PinchToZoomContainer` wrapping an [`Image`](xref:Xamarin.Forms.Image) element:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:PinchGesture;assembly=PinchGesture"
             x:Class="PinchGesture.HomePage">
    <ContentPage.Content>
        <Grid Padding="20">
            <local:PinchToZoomContainer>
                <local:PinchToZoomContainer.Content>
                    <Image Source="waterfront.jpg" />
                </local:PinchToZoomContainer.Content>
            </local:PinchToZoomContainer>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

The following code example shows how the `PinchToZoomContainer` wraps an [`Image`](xref:Xamarin.Forms.Image) element in a C# page:

```csharp
public class HomePageCS : ContentPage
{
  public HomePageCS ()
  {
    Content = new Grid {
      Padding = new Thickness (20),
      Children = {
        new PinchToZoomContainer {
          Content = new Image { Source = ImageSource.FromFile ("waterfront.jpg") }
        }
      }
    };
  }
}
```

When the [`Image`](xref:Xamarin.Forms.Image) element receives a pinch gesture, the displayed image will be zoomed-in or out. The zoom is performed by the `PinchZoomContainer.OnPinchUpdated` method, which is shown in the following code example:

```csharp
void OnPinchUpdated (object sender, PinchGestureUpdatedEventArgs e)
{
  if (e.Status == GestureStatus.Started) {
    // Store the current scale factor applied to the wrapped user interface element,
    // and zero the components for the center point of the translate transform.
    startScale = Content.Scale;
    Content.AnchorX = 0;
    Content.AnchorY = 0;
  }
  if (e.Status == GestureStatus.Running) {
    // Calculate the scale factor to be applied.
    currentScale += (e.Scale - 1) * startScale;
    currentScale = Math.Max (1, currentScale);

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the X pixel coordinate.
    double renderedX = Content.X + xOffset;
    double deltaX = renderedX / Width;
    double deltaWidth = Width / (Content.Width * startScale);
    double originX = (e.ScaleOrigin.X - deltaX) * deltaWidth;

    // The ScaleOrigin is in relative coordinates to the wrapped user interface element,
    // so get the Y pixel coordinate.
    double renderedY = Content.Y + yOffset;
    double deltaY = renderedY / Height;
    double deltaHeight = Height / (Content.Height * startScale);
    double originY = (e.ScaleOrigin.Y - deltaY) * deltaHeight;

    // Calculate the transformed element pixel coordinates.
    double targetX = xOffset - (originX * Content.Width) * (currentScale - startScale);
    double targetY = yOffset - (originY * Content.Height) * (currentScale - startScale);

    // Apply translation based on the change in origin.
    Content.TranslationX = targetX.Clamp (-Content.Width * (currentScale - 1), 0);
    Content.TranslationY = targetY.Clamp (-Content.Height * (currentScale - 1), 0);

    // Apply scale factor.
    Content.Scale = currentScale;
  }
  if (e.Status == GestureStatus.Completed) {
    // Store the translation delta's of the wrapped user interface element.
    xOffset = Content.TranslationX;
    yOffset = Content.TranslationY;
  }
}
```

This method updates the zoom level of the wrapped user interface element based on the user's pinch gesture. This is achieved by using the values of the [`Scale`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Scale), [`ScaleOrigin`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.ScaleOrigin) and [`Status`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs.Status) properties of the [`PinchGestureUpdatedEventArgs`](xref:Xamarin.Forms.PinchGestureUpdatedEventArgs) instance to calculate the scale factor to be applied at the origin of the pinch gesture. The wrapped user element is then zoomed at the origin of the pinch gesture by setting its [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX), [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY), and [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) properties to the calculated values.

## Related Links

- [PinchGesture (sample)](/samples/xamarin/xamarin-forms-samples/workingwithgestures-pinchgesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [PinchGestureRecognizer](xref:Xamarin.Forms.PinchGestureRecognizer)