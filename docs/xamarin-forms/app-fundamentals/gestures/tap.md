---
title: "Add a tap gesture recognizer"
description: "This article explains how to use the tap gesture for tap detection in a Xamarin.Forms application. Tap detection is implemented with the TapGestureRecognizer class."
ms.service: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Add a tap gesture recognizer

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/workingwithgestures-tapgesture)

_The tap gesture is used for tap detection and is implemented with the TapGestureRecognizer class._

To make a user interface element clickable with the tap gesture, create a [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) instance, handle the [`Tapped`](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) event and add the new gesture recognizer to the [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) collection on the user interface element. The following code example shows a `TapGestureRecognizer` attached to an [`Image`](xref:Xamarin.Forms.Image) element:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

By default the image will respond to single taps. Set the [`NumberOfTapsRequired`](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) property to wait for a double-tap (or more taps if required).

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

When [`NumberOfTapsRequired`](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) is set above one, the event handler will only be executed if the taps occur within a set period of time (this period is not configurable). If the second (or subsequent) taps do not occur within that period they are effectively ignored and the 'tap count' restarts.

## Using Xaml

A gesture recognizer can be added to a control in Xaml using attached properties. The syntax to add a [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) to an image is shown below (in this case defining a *double tap* event):

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

The code for the event handler (in the sample) increments a counter and changes the image from color to black &amp; white.

```csharp
void OnTapGestureRecognizerTapped(object sender, EventArgs args)
{
    tapCount++;
    var imageSender = (Image)sender;
    // watch the monkey go from color to black&white!
    if (tapCount % 2 == 0) {
        imageSender.Source = "tapped.jpg";
    } else {
        imageSender.Source = "tapped_bw.jpg";
    }
}
```

## Using ICommand

Applications that use the Model-View-ViewModel (MVVM) pattern typically use `ICommand` rather than wiring up event handlers directly. The [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) can easily support `ICommand` either by setting the binding in code:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

or using Xaml:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

The complete code for this view model can be found in the sample. The relevant `Command` implementation details are shown below:

```csharp
public class TapViewModel : INotifyPropertyChanged
{
    int taps = 0;
    ICommand tapCommand;
    public TapViewModel () {
        // configure the TapCommand with a method
        tapCommand = new Command (OnTapped);
    }
    public ICommand TapCommand {
        get { return tapCommand; }
    }
    void OnTapped (object s)  {
        taps++;
        Debug.WriteLine ("parameter: " + s);
    }
    //region INotifyPropertyChanged code omitted
}
```

## Related Links

- [TapGesture (sample)](/samples/xamarin/xamarin-forms-samples/workingwithgestures-tapgesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer](xref:Xamarin.Forms.TapGestureRecognizer)