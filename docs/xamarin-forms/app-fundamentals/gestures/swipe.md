---
title: "Add a swipe gesture recognizer"
description: "This article explains how to recognize a swipe gesture occurring on a view."
ms.service: xamarin
ms.assetid: 164976C2-1429-49FB-9EB6-621E2681C19B
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/14/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Add a swipe gesture recognizer

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)

_A swipe gesture occurs when a finger is moved across the screen in a horizontal or vertical direction, and is often used to initiate navigation through content. The code examples in this article are taken from the [Swipe Gesture](/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture) sample._

To make a [`View`](xref:Xamarin.Forms.View) recognize a swipe gesture, create a [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) instance, set the [`Direction`](xref:Xamarin.Forms.SwipeGestureRecognizer.Direction) property to a [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection) enumeration value (`Left`, `Right`, `Up`, or `Down`), optionally set the [`Threshold`](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) property, handle the [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) event, and add the new gesture recognizer to the [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) collection on the view. The following code example shows a `SwipeGestureRecognizer` attached to a [`BoxView`](xref:Xamarin.Forms.BoxView):

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

Here is the equivalent C# code:

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
```

The [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) class also includes a [`Threshold`](xref:Xamarin.Forms.SwipeGestureRecognizer.Threshold) property, that can be optionally set to a `uint` value that represents the minimum swipe distance that must be achieved for a swipe to be recognized, in device-independent units. The default value of this property is 100, meaning that any swipes that are less than 100 device-independent units will be ignored.

## Recognizing the swipe direction

In the examples above, the [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) property is set to single a value from the [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection) enumeration. However, it's also possible to set this property to multiple values from the `SwipeDirection` enumeration, so that the [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) event is fired in response to a swipe in more than one direction. However, the constraint is that a single [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) can only recognize swipes that occur on the same axis. Therefore, swipes that occur on the horizontal axis can be recognized by setting the `Direction` property to `Left` and `Right`:

```xaml
<SwipeGestureRecognizer Direction="Left,Right" Swiped="OnSwiped"/>
```

Similarly, swipes that occur on the vertical axis can be recognized by setting the [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) property to `Up` and `Down`:

```csharp
var swipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up | SwipeDirection.Down };
```

Alternatively, a [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) for each swipe direction can be created to recognize swipes in every direction:

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Right" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Up" Swiped="OnSwiped"/>
        <SwipeGestureRecognizer Direction="Down" Swiped="OnSwiped"/>
    </BoxView.GestureRecognizers>
</BoxView>
```

Here is the equivalent C# code:

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left };
leftSwipeGesture.Swiped += OnSwiped;
var rightSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Right };
rightSwipeGesture.Swiped += OnSwiped;
var upSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Up };
upSwipeGesture.Swiped += OnSwiped;
var downSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Down };
downSwipeGesture.Swiped += OnSwiped;

boxView.GestureRecognizers.Add(leftSwipeGesture);
boxView.GestureRecognizers.Add(rightSwipeGesture);
boxView.GestureRecognizers.Add(upSwipeGesture);
boxView.GestureRecognizers.Add(downSwipeGesture);
```

> [!NOTE]
> In the above examples, the same event handler responds to the [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) event firing. However, each [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) instance can use a different event handler if required.

## Responding to the swipe

An event handler for the [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) event is shown in the following example:

```csharp
void OnSwiped(object sender, SwipedEventArgs e)
{
    switch (e.Direction)
    {
        case SwipeDirection.Left:
            // Handle the swipe
            break;
        case SwipeDirection.Right:
            // Handle the swipe
            break;
        case SwipeDirection.Up:
            // Handle the swipe
            break;
        case SwipeDirection.Down:
            // Handle the swipe
            break;
    }
}
```

The [`SwipedEventArgs`](xref:Xamarin.Forms.SwipedEventArgs) can be examined to determine the direction of the swipe, with custom logic responding to the swipe as required. The direction of the swipe can be obtained from the [`Direction`](xref:Xamarin.Forms.SwipedEventArgs.Direction) property of the event arguments, which will be set to one of the values of the [`SwipeDirection`](xref:Xamarin.Forms.SwipeDirection) enumeration. In addition, the event arguments also have a [`Parameter`](xref:Xamarin.Forms.SwipedEventArgs.Parameter) property that will be set to the value of the [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) property, if defined.

## Using commands

The [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) class also includes [`Command`](xref:Xamarin.Forms.SwipeGestureRecognizer.Command) and [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) properties. These properties are typically used in applications that use the Model-View-ViewModel (MVVM) pattern. The `Command` property defines the `ICommand` to be invoked when a swipe gesture is recognized, with the `CommandParameter` property defining an object to be passed to the `ICommand.` The following code example shows how to bind the `Command` property to an `ICommand` defined in the view model whose instance is set as the page [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext):

```csharp
var boxView = new BoxView { Color = Color.Teal, ... };
var leftSwipeGesture = new SwipeGestureRecognizer { Direction = SwipeDirection.Left, CommandParameter = "Left" };
leftSwipeGesture.SetBinding(SwipeGestureRecognizer.CommandProperty, "SwipeCommand");
boxView.GestureRecognizers.Add(leftSwipeGesture);
```

The equivalent XAML code is:

```xaml
<BoxView Color="Teal" ...>
    <BoxView.GestureRecognizers>
        <SwipeGestureRecognizer Direction="Left" Command="{Binding SwipeCommand}" CommandParameter="Left" />
    </BoxView.GestureRecognizers>
</BoxView>
```

`SwipeCommand` is a property of type `ICommand` defined in the view model instance that is set as the page [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext). When a swipe gesture is recognized, the `Execute` method of the `SwipeCommand` object will be executed. The argument to the `Execute` method is the value of the [`CommandParameter`](xref:Xamarin.Forms.SwipeGestureRecognizer.CommandParameter) property. For more information about commands, see [The Command Interface](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

## Creating a swipe container

The `SwipeContainer` class, which is shown in the following code example, is a generalized swipe recognition class that be wrapped around a [`View`](xref:Xamarin.Forms.View) to perform swipe gesture recognition:

```csharp
public class SwipeContainer : ContentView
{
    public event EventHandler<SwipedEventArgs> Swipe;

    public SwipeContainer()
    {
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Left));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Right));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Up));
        GestureRecognizers.Add(GetSwipeGestureRecognizer(SwipeDirection.Down));
    }

    SwipeGestureRecognizer GetSwipeGestureRecognizer(SwipeDirection direction)
    {
        var swipe = new SwipeGestureRecognizer { Direction = direction };
        swipe.Swiped += (sender, e) => Swipe?.Invoke(this, e);
        return swipe;
    }
}
```

The `SwipeContainer` class creates [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) objects for all four swipe directions, and attaches `Swipe` event handlers. These event handlers invoke the `Swipe` event defined by the `SwipeContainer`.

The following XAML code example shows the `SwipeContainer` class wrapping a [`BoxView`](xref:Xamarin.Forms.BoxView):

```xaml
<ContentPage ...>
    <StackLayout>
        <local:SwipeContainer Swipe="OnSwiped" ...>
            <BoxView Color="Teal" ... />
        </local:SwipeContainer>
    </StackLayout>
</ContentPage>
```

The following code example shows how the `SwipeContainer` wraps a [`BoxView`](xref:Xamarin.Forms.BoxView) in a C# page:

```csharp
public class SwipeContainerPageCS : ContentPage
{
    public SwipeContainerPageCS()
    {
        var boxView = new BoxView { Color = Color.Teal, ... };
        var swipeContainer = new SwipeContainer { Content = boxView, ... };
        swipeContainer.Swipe += (sender, e) =>
        {
          // Handle the swipe
        };

        Content = new StackLayout
        {
            Children = { swipeContainer }
        };
    }
}
```

When the [`BoxView`](xref:Xamarin.Forms.BoxView) receives a swipe gesture, the [`Swiped`](xref:Xamarin.Forms.SwipeGestureRecognizer.Swiped) event in the [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) is fired. This is handled by the `SwipeContainer` class, which fires its own `Swipe` event. This `Swipe` event is handled on the page. The [`SwipedEventArgs`](xref:Xamarin.Forms.SwipedEventArgs) can then be examined to determine the direction of the swipe, with custom logic responding to the swipe as required.

## Related links

- [Swipe Gesture (sample)](/samples/xamarin/xamarin-forms-samples/workingwithgestures-swipegesture)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [SwipeGestureRecognizer](xref:Xamarin.Forms.SwipeGestureRecognizer)