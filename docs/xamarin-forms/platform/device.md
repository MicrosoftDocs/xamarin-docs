---
title: "Xamarin.Forms Device Class"
description: "This article explains how to use the Xamarin.Forms Device class, for fine-grained control over functionality and layouts on a per-platform basis."
ms.service: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/20/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Device Class

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/workingwithdevice)

The [`Device`](xref:Xamarin.Forms.Device) class contains a number of properties and methods to help developers customize layout and functionality on a per-platform basis.

In addition to methods and properties to target code at specific hardware types and sizes, the `Device` class includes methods that can be used to interact with UI controls from background threads. For more information, see [Interact with the UI from background threads](#interact-with-the-ui-from-background-threads).

## Provide platform-specific values

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

Alternatively, the `OnPlatform` markup extension can be used in XAML to customize UI appearance on a per-platform basis. For more information, see [OnPlatform Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## Device.Idiom

The `Device.Idiom` property can be used to alter layouts or functionality depending on the device the application is running on. The [`TargetIdiom`](xref:Xamarin.Forms.TargetIdiom) enumeration contains the following values:

- **Phone** – iPhone, iPod touch, and Android devices narrower than 600 dips^
- **Tablet** – iPad, Windows devices, and Android devices wider than 600 dips^
- **Desktop** – only returned in [UWP apps](~/xamarin-forms/platform/windows/installation/index.md) on Windows 10 desktop computers (returns `Phone` on mobile Windows devices, including in Continuum scenarios)
- **TV** – Tizen TV devices
- **Watch** – Tizen watch devices
- **Unsupported** – unused

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

Alternatively, the `OnIdiom` markup extension can be used in XAML to customize UI appearance based on the idiom of the device the application is running on. For more information, see [OnIdiom Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom-markup-extension).

## Device.FlowDirection

The [`Device.FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) value retrieves a [`FlowDirection`](xref:Xamarin.Forms.FlowDirection) enumeration value that represents the current flow direction being used by the device. Flow direction is the direction in which the UI elements on the page are scanned by the eye. The enumeration values are:

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
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

## Device.Styles

The [`Styles` property](~/xamarin-forms/user-interface/styles/index.md) contains built-in style definitions that can be applied to some controls' (such as `Label`) `Style` property. The available styles are:

- BodyStyle
- CaptionStyle
- ListItemDetailTextStyle
- ListItemTextStyle
- SubtitleStyle
- TitleStyle

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

## Device.GetNamedColor

Xamarin.Forms 4.6 introduces support for named colors. A named color is a color that has a different value depending on which system mode (for example, light or dark) is active on the device. On Android, named colors are accessed via the [R.Color](https://developer.android.com/reference/android/R.color#constants_2) class. On iOS, named colors are called [system colors](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/#system-colors). On the Universal Windows Platform, named colors are called [XAML theme resources](/windows/uwp/design/controls-and-patterns/xaml-theme-resources).

The `GetNamedColor` method can be used to retrieve named colors on Android, iOS, and UWP. The method takes a `string` argument and returns a [`Color`](xref:Xamarin.Forms.Color):

```csharp
// Retrieve an Android named color
Color color = Device.GetNamedColor(NamedPlatformColor.HoloBlueBright);
```

`Color.Default` will be returned when a color name cannot be found, or when `GetNamedColor` is invoked on an unsupported platform.

> [!NOTE]
> Because the `GetNamedColor` method returns a `Color` that's specific to a platform, it should typically be used in conjunction with the [`Device.RuntimePlatform`](xref:Xamarin.Forms.Device.RuntimePlatform) property.

The `NamedPlatformColor` class contains the constants that define the named colors for Android, iOS, and UWP:

| Android | iOS | macOS | UWP |
| --- | --- | --- | --- |
| `BackgroundDark` | `Label` | `AlternateSelectedControlTextColor` | `SystemAltHighColor` |
| `BackgroundLight` | `Link` | `ControlAccent` | `SystemAltLowColor` |
| `Black` | `OpaqueSeparator` | `ControlBackgroundColor` | `SystemAltMediumColor` |
| `DarkerGray` | `PlaceholderText` | `ControlColor` | `SystemAltMediumHighColor` |
| `HoloBlueBright` | `QuaternaryLabel` | `DisabledControlTextColor` | `SystemAltMediumLowColor` |
| `HoloBlueDark` | `SecondaryLabel` | `FindHighlightColor` | `SystemBaseHighColor` |
| `HoloBlueLight` | `Separator` | `GridColor` | `SystemBaseLowColor` |
| `HoloGreenDark` | `SystemBlue` | `HeaderTextColor` | `SystemBaseMediumColor` |
| `HoloGreenLight` | `SystemGray` | `HighlightColor` | `SystemBaseMediumHighColor` |
| `HoloOrangeDark` | `SystemGray2` | `KeyboardFocusIndicatorColor` | `SystemBaseMediumLowColor` |
| `HoloOrangeLight` | `SystemGray3` | `Label` | `SystemChromeAltLowColor` |
| `HoloPurple` | `SystemGray4` | `LabelColor` | `SystemChromeBlackHighColor` |
| `HoloRedDark` | `SystemGray5` | `Link` | `SystemChromeBlackLowColor` |
| `HoloRedLight` | `SystemGray6` | `LinkColor` | `SystemChromeBlackMediumColor` |
| `TabIndicatorText` | `SystemGreen` | `PlaceholderText` | `SystemChromeBlackMediumLowColor` |
| `Transparent` | `SystemIndigo` | `PlaceholderTextColor` | `SystemChromeDisabledHighColor` |
| `White` | `SystemOrange` | `QuaternaryLabel`| `SystemChromeDisabledLowColor` |
| `WidgetEditTextDark` | `SystemPink` | `QuaternaryLabelColor` | `SystemChromeHighColor` |
| | `SystemPurple` | `SecondaryLabel` | `SystemChromeLowColor` |
| | `SystemRed` | `SecondaryLabelColor` | `SystemChromeMediumColor` |
| | `SystemTeal` | `SelectedContentBackgroundColor` | `SystemChromeMediumLowColor` |
| | `SystemYellow` | `SelectedControlColor` | `SystemChromeWhiteColor` |
| | `TertiaryLabel` | `SelectedControlTextColor` | `SystemListLowColor` |
| | | `SelectedMenuItemTextColor` | `SystemListMediumColor`|
| | | `SelectedTextBackgroundColor` | |
| | | `SelectedTextColor` | |
| | | `Separator` | |
| | | `SeparatorColor` | |
| | | `ShadowColor` | |
| | | `SystemBlue` | |
| | | `SystemGray` | |
| | | `SystemGreen` | |
| | | `SystemIndigo` | |
| | | `SystemOrange` | |
| | | `SystemPink` | |
| | | `SystemPurple` | |
| | | `SystemRed` | |
| | | `SystemTeal` | |
| | | `SystemYellow` | |
| | | `TertiaryLabel` | |
| | | `TertiaryLabelColor` | |
| | | `TextBackgroundColor` | |
| | | `TextColor` | |
| | | `UnderPageBackgroundColor` | |
| | | `UnemphasizedSelectedContentBackgroundColor` | |
| | | `UnemphasizedSelectedTextBackgroundColor` | |
| | | `UnemphasizedSelectedTextColor` | |
| | | `WindowBackgroundColor` | |
| | | `WindowFrameTextColor` | |

## Device.StartTimer

The `Device` class also has a `StartTimer` method which provides a simple way to trigger time-dependent tasks that works in Xamarin.Forms common code, including a .NET Standard library. Pass a `TimeSpan` to set the interval and return `true` to keep the timer running or `false` to stop it after the current invocation.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () =>
{
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

If the code inside the timer interacts with the user-interface (such as setting the text of a `Label` or displaying an alert) it should be done inside a `BeginInvokeOnMainThread` expression (see below).

> [!NOTE]
> The `System.Timers.Timer` and `System.Threading.Timer` classes are .NET Standard alternatives to using the `Device.StartTimer` method.

## Interact with the UI from background threads

Most operating systems, including iOS, Android, and the Universal Windows Platform, use a single-threading model for code involving the user interface. This thread is often called the *main thread* or the *UI thread*. A consequence of this model is that all code that accesses user interface elements must run on the application's main thread.

Applications sometimes use background threads to perform potentially long running operations, such as retrieving data from a web service. If code running on a background thread needs to access user interface elements, it must run that code on the main thread.

The `Device` class includes the following `static` methods that can be used to interact with user interface elements from backgrounds threads:

| Method | Arguments | Returns | Purpose |
|---|---|---|---|
| `BeginInvokeOnMainThread` | `Action` | `void` | Invokes an `Action` on the main thread, and doesn't wait for it to complete. |
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | Invokes a `Func<T>` on the main thread, and waits for it to complete. |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | Invokes an `Action` on the main thread, and waits for it to complete. |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | Invokes a `Func<Task<T>>` on the main thread, and waits for it to complete. |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | Invokes a `Func<Task>` on the main thread, and waits for it to complete. |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | Returns the `SynchronizationContext` for the main thread. |

The following code shows an example of using the `BeginInvokeOnMainThread` method:

```csharp
Device.BeginInvokeOnMainThread (() =>
{
    // interact with UI elements
});
```

## Related links

- [Device Sample](/samples/xamarin/xamarin-forms-samples/workingwithdevice)
- [Styles Sample](/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [Device API](xref:Xamarin.Forms.Device)