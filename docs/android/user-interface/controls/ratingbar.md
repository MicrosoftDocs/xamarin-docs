---
title: "Xamarin.Android RatingBar"
description: "How to add a RatingBar widget to an Android activity."
ms.service: xamarin
ms.assetid: d7a1f9bb-926d-4f93-9e8e-0fa933e330e7
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/29/2018
---

# Xamarin.Android RatingBar

A RatingBar is a UI widget that displays a rating from one to five stars. The user may select a rating by taping on a star
In this section, you'll create a widget that allows the user to provide a
rating, with the [`RatingBar`](xref:Android.Widget.RatingBar) widget.

![Example of a RatingBar](ratingbar-images/01-ratingbar.png)

## Creating a RatingBar

1. Open the **Resource/layout/Main.axml** file and add the
   [`RatingBar`](xref:Android.Widget.RatingBar)
   element (inside the [`LinearLayout`](xref:Android.Widget.LinearLayout)):

   ```xml
   <RatingBar android:id="@+id/ratingbar"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:numStars="5"
            android:stepSize="1.0"/>
   ```

   The `android:numStars` attribute defines how many stars to display
   for the rating bar. The `android:stepSize` attribute defines the
   granularity for each star (for example, a value of `0.5` would allow
   half-star ratings).

2. To do something when a new rating has been set, add the following
   code to the end of the
   [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
   method:

    ```csharp
    RatingBar ratingbar = FindViewById<RatingBar>(Resource.Id.ratingbar);

    ratingbar.RatingBarChange += (o, e) => {
            Toast.MakeText(this, "New Rating: " + ratingbar.Rating.ToString (), ToastLength.Short).Show ();
    };
    ```

    This captures the [`RatingBar`](xref:Android.Widget.RatingBar) widget from
    the layout with [`FindViewById`](xref:Android.App.Activity.FindViewById*)
    and then sets an event method then defines the action to perform when the user
    sets a rating. In this case, a simple [`Toast`](xref:Android.Widget.Toast)
    message displays the new rating.

3. Run the application.
