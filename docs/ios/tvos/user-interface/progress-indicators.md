---
title: "Working with tvOS progress indicators in Xamarin"
description: "This document describes how to work with progress indicators in a tvOS app built with Xamarin. It discusses both progress bars and activity indicators."
ms.service: xamarin
ms.assetid: 582B6D0C-1F16-4299-A9A6-5651E76009FE
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
no-loc: [Objective-C]
---

# Working with tvOS progress indicators in Xamarin

_This article covers designing and working with progress indicators inside
of a Xamarin.tvOS app._

There might be times when your Xamarin.tvOS app needs to load new content or
perform a lengthy processing operation. During these times, you should
present either an activity indicator or a progress bar to let the user know
that the app is still running and to give them some indication as to the
length of the task being run.

![Sample progress indicators](progress-indicators-images/intro01.png "Sample
progress indicators")

## About activity indicators

An activity indicator presents as a spinning cog and is used to represent a
task of an undetermined length. The indicator is presented when the task
starts and disappears when the task is completed.

Apple has the following suggestions for working with activity indicators:

- **Whenever possible, use progress bars instead** - Because an activity
  indicator gives the user no feedback as to how long the process being run
  will take, always use a progress bar if the length is known (for example,
  how many bytes to download in a file).
- **Keep the indicator animated** - Users relate a stationary activity
  indicator to a stalled app, so you should always animate the indicator
  while it is being displayed.
- **Describe the task being processed** - Just displaying the activity
  indicator by itself isn't enough; the user needs to be informed about the
  process on which they are waiting. Include a meaningful label (usually a 
  single, complete sentence) that clearly defines the task.

## About progress bars

A progress bar presents as a line that fills with color to indicate the
state and length of a time-consuming task. Progress bars should always be
used when the length of the tasks is known or can be computed.

Apple has the following suggestions for working with progress bars:

- **Accurately report progress** - Progress bars should always present an
  accurate representation of the time required to complete a task. Never
  misrepresent the time to make the app appear busy.
- **Use for well-defined durations** - Progress bars should not only show
  that a lengthy task is taking place, but give the user and indication of
  how much of the task is completed and an estimate of the time remaining.

## Progress indicators and storyboards

The easiest way to work with a progress indicator in a Xamarin.tvOS app is
to add it to the app's UI using the iOS Designer.

# [Visual Studio for Mac](#tab/macos)

1. In the **Solution Pad**, double-click the **Main.storyboard** file and
   open it for editing.

2. Drag an **Activity Indicator** from the **Toolbox** and drop it on the
   view: 

    ![An activity indicator](progress-indicators-images/activity01.png "An
     activity indicator")

3. In the **Widget** tab of the **Properties Pad**, you can adjust several
   properties of the activity indicator such as its **Style**,
   **Behavior**, and **Name**: 

    ![The Widget tab for an activity
    indicator](progress-indicators-images/activity02.png "The Widget tab for
    an activity indicator")
    
    The **Name** determines the name of the property that represents the
    activity indicator in C# code.

4. Drag a **Progress View** from the **Toolbox** and drop it on the view: 

    ![A progress view](progress-indicators-images/activity03.png "A progress
    view")

5. In the **Widget** tab of the **Property Explorer**, you can adjust
   several properties of the progress view such as its **Style**,
   **Progress** (percent complete), and **Name**: 

    ![The Widget tab for a progress
    view](progress-indicators-images/activity04.png "The Widget tab for a
    progress view")
    
    The **Name** determines the name of the property that represents the
    progress view in C# code.

6. Save your changes.

# [Visual Studio](#tab/windows)

1. In the **Solution Explorer**, double-click the **Main.storyboard** file 
   and open it for editing.

2. Drag an **Activity Indicator** from the **Toolbox** and drop it on the
   view: 

    ![An activity indicator](progress-indicators-images/activity01-vs.png
    "An activity indicator")

3. In the **Widget** tab of the **Properties Explorer**, you can adjust
   several properties of the activity indicator such as its **Style**,
   **Behavior**, and **Name**: 

    ![The Widget tab for an activity
    indicator](progress-indicators-images/activity02-vs.png "The Widget tab
    for an activity indicator")

    The **Name** determines the name of the property that represents
    the activity indicator in C# code.

4. Drag a **Progress View** from the **Toolbox** and drop it on the view: 

   ![A progress view](progress-indicators-images/activity03-vs.png "A
   progress view")

5. In the **Widget** tab of the **Property Explorer**, you can adjust
   several properties of the progress view such as its **Style**,
   **Progress** (percent complete), and **Name**: 

    ![The Widget tab for a progress
    view](progress-indicators-images/activity04-vs.png "The Widget tab for a
    progress view")
    
    The **Name** determines the name of the property that represents the
    progress view in C# code.

6. Save your changes.

-----

For more information on working with storyboards, please see our [Hello,
tvOS Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md). 

## Working with activity indicators

As stated above, activity indicators should be shown when your app is
running a long process of indeterminate length.

At any point, you can see if an activity indicator is animating by checking
its `IsAnimating` property. If the `HidesWhenStopped` property is `true`,
the activity indicator will automatically be hidden when its animation is
stopped.

You can use the following code to start the animation: 

```csharp
ActivityIndicator.StartAnimating();
```

And the following will stop the animation:

```csharp
ActivityIndicator.StopAnimating();
```

> [!NOTE]
> These code snippets assume that the activity indicator's **Name**
> was set to **ActivityIndicator** in the **Widget** tab of the iOS
> Designer.

## Working with progress bars

Again, a progress bar should be used any time your app is executing a long
running task of a known duration. 

The `Progress` property is used to set the amount of the task that has been
completed from 0% to 100% (0.0 to 1.0). Use the `ProgressTintColor` property
to set the color of the amount completed bar and the `TrackTintColor`
property to set the background color (uncompleted amount).

## Summary

This article has covered designing and working with progress indicators
inside of a Xamarin.tvOS app.

## Related links

- [tvOS Samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2btvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/design/human-interface-guidelines/designing-for-tvos)
- [App Programming Guide for tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)