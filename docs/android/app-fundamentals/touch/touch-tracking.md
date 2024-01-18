---
title: "Multi-Touch Finger Tracking in Xamarin.Android"
description: "This topic demonstrates how to track touch events from multiple fingers"
ms.service: xamarin
ms.assetid: 048D51F9-BD6C-4B44-8C53-CCEF276FC5CC
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
---

# Multi-Touch Finger Tracking

_This topic demonstrates how to track touch events from multiple fingers_

There are times when a multi-touch application needs to track
individual fingers as they move simultaneously on the screen. One
typical application is a finger-paint program. You want the user to be
able to draw with a single finger, but also to draw with multiple
fingers at once. As your program processes multiple touch events, it
needs to distinguish which events correspond to each finger. Android
supplies an ID code for this purpose, but obtaining and handling that
code can be a little tricky.

For all the events associated with a particular finger, the ID code
remains the same. The ID code is assigned when a finger first touches
the screen, and becomes invalid after the finger lifts from the screen.
These ID codes are generally very small integers, and Android reuses
them for later touch events.

Almost always, a program that tracks individual fingers maintains a
dictionary for touch tracking. The dictionary key is the ID code that
identifies a particular finger. The dictionary value depends on the
application. In the
[FingerPaint](/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
program, each finger stroke (from touch to release) is associated with
an object that contains all the information necessary to render the
line drawn with that finger. The program defines a small
`FingerPaintPolyline` class for this purpose:

```csharp
class FingerPaintPolyline
{
    public FingerPaintPolyline()
    {
        Path = new Path();
    }

    public Color Color { set; get; }

    public float StrokeWidth { set; get; }

    public Path Path { private set; get; }
}
```

Each polyline has a color, a stroke width, and an Android graphics
[`Path`](xref:Android.Graphics.Path) object to accumulate and
render multiple points of the line as it's being drawn.

The remainder of the code shown below is contained in a `View`
derivative named `FingerPaintCanvasView`. That class maintains a
dictionary of objects of type `FingerPaintPolyline` during the time
that they are actively being drawn by one or more fingers:

```csharp
Dictionary<int, FingerPaintPolyline> inProgressPolylines = new Dictionary<int, FingerPaintPolyline>();
```

This dictionary allows the view to quickly obtain the
`FingerPaintPolyline` information associated with a particular finger.

The `FingerPaintCanvasView` class also maintains a `List` object for
the polylines that have been completed:

```csharp
List<FingerPaintPolyline> completedPolylines = new List<FingerPaintPolyline>();
```

The objects in this `List` are in the same order that they were drawn.

`FingerPaintCanvasView` overrides two methods defined by `View`:
[`OnDraw`](xref:Android.Views.View.OnDraw*)
and
[`OnTouchEvent`](xref:Android.Views.View.OnTouchEvent*).
In its `OnDraw` override, the view draws the completed polylines and
then draws the in-progress polylines.

The override of the `OnTouchEvent` method begins by obtaining a
`pointerIndex` value from the `ActionIndex` property. This
`ActionIndex` value differentiates between multiple fingers, but it is
not consistent across multiple events. For that reason, you use the
`pointerIndex` to obtain the pointer `id` value from the `GetPointerId`
method. This ID *is* consistent across multiple events:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        // ...
    }

    // Invalidate to update the view
    Invalidate();

    // Request continued touch input
    return true;
}
```

Notice that the override uses the `ActionMasked` property in the
`switch` statement rather than the `Action` property. Here's why:

When you're dealing with multi-touch, the `Action` property has a value
of `MotionEventsAction.Down` for the first finger to touch the screen,
and then values of `Pointer2Down` and `Pointer3Down` as the second and
third fingers also touch the screen. As the fourth and fifth fingers
make contact, the `Action` property has numeric values that don't even
correspond to members of the `MotionEventsAction` enumeration! You'd
need to examine the values of bit flags in the values to interpret what
they mean.

Similarly, as the fingers leave contact with the screen, the `Action`
property has values of `Pointer2Up` and `Pointer3Up` for the second and
third fingers, and `Up` for the first finger.

The `ActionMasked` property takes on a fewer number of values because
it's intended to be used in conjunction with the `ActionIndex` property
to differentiate between multiple fingers. When fingers touch the
screen, the property can only equal `MotionEventActions.Down` for the
first finger and `PointerDown` for subsequent fingers. As the fingers
leave the screen, `ActionMasked` has values of `Pointer1Up` for the
subsequent fingers and `Up` for the first finger.

When using `ActionMasked`, the `ActionIndex` distinguishes among the
subsequent fingers to touch and leave the screen, but you usually don't
need to use that value except as an argument to other methods in the
`MotionEvent` object. For multi-touch, one of the most important of
these methods is `GetPointerId` called in the code above. That method
returns a value that you can use for a dictionary key to associate
particular events to fingers.

The `OnTouchEvent` override in the
[FingerPaint](/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
program processes the `MotionEventActions.Down` and `PointerDown`
events identically by creating a new `FingerPaintPolyline` object and
adding it to the dictionary:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // Get the pointer index
    int pointerIndex = args.ActionIndex;

    // Get the id to identify a finger over the course of its progress
    int id = args.GetPointerId(pointerIndex);

    // Use ActionMasked here rather than Action to reduce the number of possibilities
    switch (args.ActionMasked)
    {
        case MotionEventActions.Down:
        case MotionEventActions.PointerDown:

            // Create a Polyline, set the initial point, and store it
            FingerPaintPolyline polyline = new FingerPaintPolyline
            {
                Color = StrokeColor,
                StrokeWidth = StrokeWidth
            };

            polyline.Path.MoveTo(args.GetX(pointerIndex),
                                 args.GetY(pointerIndex));

            inProgressPolylines.Add(id, polyline);
            break;
        // ...
    }
    // ...        
}
```

Notice that the `pointerIndex` is also used to obtain the position of
the finger within the view. All the touch information is associated
with the `pointerIndex` value. The `id` uniquely identifies fingers
across multiple messages, so that's used to create the dictionary
entry.

Similarly, the `OnTouchEvent` override also handles the
`MotionEventActions.Up` and `Pointer1Up` identically by transferring
the completed polyline to the `completedPolylines` collection so they
can be drawn during the `OnDraw` override. The code also removes the
`id` entry from the dictionary:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Up:
        case MotionEventActions.Pointer1Up:

            inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                args.GetY(pointerIndex));

            // Transfer the in-progress polyline to a completed polyline
            completedPolylines.Add(inProgressPolylines[id]);
            inProgressPolylines.Remove(id);
            break;

        case MotionEventActions.Cancel:
            inProgressPolylines.Remove(id);
            break;
    }
    // ...        
}
```

Now for the tricky part.

Between the down and up events, generally there are many
`MotionEventActions.Move` events. These are bundled in a single call to
`OnTouchEvent`, and they must be handled differently from the `Down`
and `Up` events. The `pointerIndex` value obtained earlier from the
`ActionIndex` property must be ignored. Instead, the method must obtain
multiple `pointerIndex` values by looping between 0 and the
`PointerCount` property, and then obtain an `id` for each of those
`pointerIndex` values:

```csharp
public override bool OnTouchEvent(MotionEvent args)
{
    // ...
    switch (args.ActionMasked)
    {
        // ...
        case MotionEventActions.Move:

            // Multiple Move events are bundled, so handle them differently
            for (pointerIndex = 0; pointerIndex < args.PointerCount; pointerIndex++)
            {
                id = args.GetPointerId(pointerIndex);

                inProgressPolylines[id].Path.LineTo(args.GetX(pointerIndex),
                                                    args.GetY(pointerIndex));
            }
            break;
        // ...
    }
    // ...        
}
```

This type of processing allows the
[FingerPaint](/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
program to track individual fingers and draw the results on the screen:

[![Example screenshot from FingerPaint example](touch-tracking-images/image01.png)](touch-tracking-images/image01.png#lightbox)

You've now seen how you can track individual fingers on the screen and
distinguish among them.

## Related Links

- [Equivalent Xamarin iOS guide](~/ios/app-fundamentals/touch/touch-tracking.md)
- [FingerPaint (sample)](/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)