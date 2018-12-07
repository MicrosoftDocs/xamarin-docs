---
title: "Xamarin.Forms Device Class"
description: "This article explains how to use the Xamarin.Forms Device class, for fine-grained control over functionality and layouts on a per-platform basis."
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2018
---

# Xamarin.Forms Device Class

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)

The [`Device`](xref:Xamarin.Forms.Device) class contains a number of properties and methods to help developers customize layout and functionality on a per-platform basis.

In addition to methods and properties to target code at specific hardware types and sizes, the `Device` class includes the [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) method which should be used when interacting with UI controls from background threads.

<a name="providing-platform-values" />

## Providing Platform-Specific Values

Prior to Xamarin.Forms 2.3.4, the platform the application was running on could be obtained by examining the [`Device.OS`](xref:Xamarin.Forms.Device.OS) property and comparing it to the [`TargetPlatform.iOS`](xref:Xamarin.Forms.TargetPlatform.iOS), [`TargetPlatform.Android`](xref:Xamarin.Forms.TargetPlatform.Android), [`TargetPlatform.WinPhone`](xref:Xamarin.Forms.TargetPlatform.WinPhone), and [`TargetPlatform.Windows`](xref:Xamarin.Forms.TargetPlatform.Windows) enumeration values. Similarly, one of the [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) overloads could be used to provide platform-specific values to a control.

However, since Xamarin.Forms 2.3.4 these APIs have been deprecated and replaced. The [`Device`](xref:Xamarin.Forms.Device) class now contains public string constants that identify platforms – [`Device.iOS`](xref:Xamarin.Forms.Device.iOS), [`Device.Android`](xref:Xamarin.Forms.Device.Android), `Device.WinPhone`(deprecated), `Device.WinRT` (deprecated), [`Device.UWP`](xref:Xamarin.Forms.Device.UWP), and [`Device.macOS`](xref:Xamarin.Forms.Device.macOS). Similarly, the  [`Device.OnPlatform`](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) overloads have been replaced with the [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) and [`On`](xref:Xamarin.Forms.On) APIs.

In C#, platform-specific values can be provided by creating a `switch` statement on the [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) property, and then providing `case` statements for the required platforms:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

The [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) and [`On`](xref:Xamarin.Forms.On) classes provide the same functionality in XAML:

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

The [`OnPlatform`](xref:Xamarin.Forms.OnPlatform`1) class is a generic class that must be instantiated with an `x:TypeArguments` attribute that matches the target type. In the [`On`](xref:Xamarin.Forms.On) class, the [`Platform`](xref:Xamarin.Forms.On.Platform) attribute can accept a single `string` value, or multiple comma-delimited `string` values.

> [!IMPORTANT]
> Providing an incorrect `Platform` attribute value in the `On` class will not result in an error. Instead, the code will execute without the platform-specific value being applied.

Alternatively, the `OnPlatform` markup extension can be used in XAML to customize UI appearance on a per-platform basis. For more information, see [OnPlatform Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform).

<a name="Device_Idiom" />

## Device.Idiom

The `Device.Idiom` property can be used to alter layouts or functionality depending on the device the application is running on. The [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom) enumeration contains the following values:

-  **Phone** – iPhone, iPod touch, and Android devices narrower than 600 dips^
-  **Tablet** – iPad, Windows devices, and Android devices wider than 600 dips^
-  **Desktop** – only returned in [UWP apps](~/xamarin-forms/platform/windows/installation/index.md) on Windows 10 desktop computers (returns `Phone` on mobile Windows devices, including in Continuum scenarios)
-  **TV** – Tizen TV devices
-  **Watch** – Tizen watch devices
-  **Unsupported** – unused

*^ dips is not necessarily the physical pixel count*

The `Idiom` property is especially useful for building layouts that take advantage of larger screens, like this:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

The [`OnIdiom`](xref:Xamarin.Forms.OnIdiom`1) class provides the same functionality in XAML:

```xaml
<StackLayout>
    <StackLayout.Margin>
        <OnIdiom x:TypeArguments="Thickness">
            <OnIdiom.Phone>0,20,0,0</OnIdiom.Phone>
            <OnIdiom.Tablet>0,40,0,0</OnIdiom.Tablet>
            <OnIdiom.Desktop>0,60,0,0</OnIdiom.Desktop>
        </OnIdiom>
    </StackLayout.Margin>
    ...
</StackLayout>
```

The [`OnIdiom`](xref:Xamarin.Forms.OnPlatform`1) class is a generic class that must be instantiated with an `x:TypeArguments` attribute that matches the target type.

Alternatively, the `OnIdiom` markup extension can be used in XAML to customize UI appearance based on the idiom of the device the application is running on. For more information, see [OnIdiom Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom).

## Device.FlowDirection

The [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) value retrieves a [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) enumeration value that represents the current flow direction being used by the device. Flow direction is the direction in which the UI elements on the page are scanned by the eye. The enumeration values are:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToRight`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

In XAML, the [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) value can be retrieved by using the `x:Static` markup extension:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

The equivalent code in C# is:

```csharp
this.FlowDirection = Device.FlowDirection;
```

For more information about flow direction, see [Right-to-left Localization](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

<a name="Device_Styles" />

## Device.Styles

The [`Styles` property](~/xamarin-forms/user-interface/styles/index.md) contains built-in style definitions that can be applied to some controls' (such as `Label`) `Style` property. The available styles are:

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## Device.GetNamedSize

`GetNamedSize` can be used when setting [`FontSize`](~/xamarin-forms/user-interface/text/fonts.md) in C# code:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

<a name="Device_OpenUri" />

## Device.OpenUri

The `OpenUri` method can be used to trigger operations on the underlying platform, such as open a URL in the native web browser (**Safari** on iOS or **Internet** on Android).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

The [WebView sample](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) includes an example using `OpenUri` to open URLs and also trigger phone calls.

The [Maps sample](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) also uses `Device.OpenUri` to display maps and directions using the native **Maps** apps on iOS and Android.

<a name="Device_StartTimer" />

## Device.StartTimer

The `Device` class also has a `StartTimer` method which provides a simple way to trigger time-dependent tasks that works in Xamarin.Forms common code, including a .NET Standard library. Pass a `TimeSpan` to set the interval and return `true` to keep the timer running or `false` to stop it after the current invocation.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

If the code inside the timer interacts with the user-interface (such as setting the text of a `Label` or displaying an alert) it should be done inside a `BeginInvokeOnMainThread` expression (see below).

<a name="Device_BeginInvokeOnMainThread" />

## Device.BeginInvokeOnMainThread

User interface elements should never be accessed by background threads, such as code running in a timer or a completion handler for asynchronous operations like web requests. Any background code that needs to update the user interface should be wrapped inside [`BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)). This is the equivalent of `InvokeOnMainThread` on iOS, `RunOnUiThread` on Android, and `Dispatcher.RunAsync` on the Universal Windows Platform.

The Xamarin.Forms code is:

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

Note that methods using `async/await` do not need to use `BeginInvokeOnMainThread` if they are running from the main UI thread.

## Summary

The Xamarin.Forms `Device` class allows fine-grained control over functionality and layouts on a per-platform basis - even in common code (either .NET Standard library projects or Shared Projects).


## Related Links

- [Device Sample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [Styles Sample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Device](xref:Xamarin.Forms.Device)
