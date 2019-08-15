---
title: "Activity Indicator in Xamarin.Forms"
description: "The ActivityIndicator control indicates to users that the application is engaged in a lengthy activity, without giving any indication of progress. This article explains how to use an ActivityIndicator in XAML and code."
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
---

# Xamarin.Forms ActivityIndicator
[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)

The Xamarin.Forms[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) is a control that displays an animation to show that the application is engaged in a lengthy activity. Unlike the [`ProgressBar`](xref:Xamarin.Forms.ProgressBar), the `ActivityIndicator` gives no indication of progress. The `ActivityIndicator` inherits from [`View`](xref:Xamarin.Forms.View).

The following screenshot shows an `ActivityIndicator` control on iOS and Android:

![Screenshot of ActivityIndicator on iOS and Android](activityindicator-images/activityindicators-default.png "Screenshot of ActivityIndicator on iOS and Android")

The `ActivityIndicator` control defines the following properties:

* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) is a `bool` value that indicates whether the `ActivityIndicator` should be visible and animating, or hidden. When the value is `false` the `ActivityIndicator` won't be visible.
* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color) is a `Color` value that defines the display color of the `ActivityIndicator`.

These properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that the `ActivityIndicator` can be styled and be the target of data bindings.

## Create an ActivityIndicator

An `ActivityIndicator` can be instantiated in XAML. Its `IsRunning` property can be set to determine if the control is visible and animating. If the `IsRunning` property isn't set, it defaults to `false` and the `ActivityIndicator` won't be visible. The following example shows how to instantiate an `ActivityIndicator` in XAML with the optional `IsRunning` property set:

```xaml
<ActivityIndicator IsRunning="true" />
```

An `ActivityIndicator` can also be created in code:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## ActivityIndicator appearance properties

The `Color` property can be set to define the `ActivityIndicator` color. The following example shows how to instantiate an `ActivityIndicator` in XAML with the `Color` property set:

```xaml
<ActivityIndicator Color="Orange" />
```

The `Color` property can also be set when creating an `ActivityIndicator` in code:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

The following screenshot shows the `ActivityIndicator` with the `Color` property set to `Color.Orange` on iOS and Android:

![Screenshot of styled ActivityIndicator on iOS and Android](activityindicator-images/activityindicators-styled.png "Screenshot of styled ActivityIndicator on iOS and Android")

## Related links

* [ActivityIndicator Demos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
