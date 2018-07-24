---
title: "Adding iOS-specific Formatting"
description: "This article explains how to set iOS-specific appearance without using a Xamarin.Forms custom renderer."
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
---

# Adding iOS-specific Formatting

One way to set iOS-specific formatting is to create a
[custom renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) for a control
and set platform-specific styles and colors for each platform.

Other options to control the way your Xamarin.Forms iOS app's appearance include:

* Configuring display options in [**Info.plist**](#info-plist)
* Setting control styles via the [`UIAppearance` API](#uiappearance)

These alternatives are discussed below.

<a name="info-plist"/>

## Customizing Info.plist

The **Info.plist** file lets you configure some aspects of an iOS
application's renderering, such as how (and whether) the status bar is
shown.

For example, the [Todo sample](https://developer.xamarin.com/samples/xamarin-forms/Todo/) uses the following
code to set the navigation bar color and text color on all platforms:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

The result is shown in the screen snippet below. Notice that the status bar
items are black (this cannot be set within Xamarin.Forms because it is
a platform-specific feature).

![](theme-images/status-default-sml.png "iOS Theming")

Ideally the status bar would also be white - something we can accomplish
directly in the iOS project. Add the following entries to the **Info.plist** to
force the status bar to be white:

![](theme-images/info-plist.png "iOS Info.plist Entries")

or edit the corresponding **Info.plist** file directly to include:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Now when the app is run, the navigation bar is green and its text is white
(due to Xamarin.Forms formatting) *and* the status bar text is also white
thanks to iOS-specific configuration:

![](theme-images/status-white-sml.png "iOS Theming")

<a name="uiappearance"/>

## UIAppearance API

The [`UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
can be used to set visual properties on many iOS controls
*without* having to create a [custom renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

Adding a single line of code to the **AppDelegate.cs** `FinishedLaunching`
method can style all controls of a given type using their `Appearance` property. The
following code contains two examples - globally styling the tab bar and switch control:

**AppDelegate.cs** in the iOS Project

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
  // tab bar
    UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
  // switch
    UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
    // required Xamarin.Forms code
    Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
```

### UITabBar

By default, the selected tab bar icon in a
[`TabbedPage`](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
would be blue:

![](theme-images/tabbar-default.png "Default iOS Tab Bar Icon in TabbedPage")

To change this behavior, set the `UITabBar.Appearance` property:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

This causes the selected tab to be green:

![](theme-images/tabbar-custom.png "Green iOS Tab Bar Icon in TabbedPage")

Using this API lets you customize the appearance of the Xamarin.Forms
`TabbedPage` on iOS with very little code. Refer to the
[Customize Tabs recipe](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
for more details on using a custom renderer to set a specific font for the tab.

### UISwitch

The `Switch` control is another example that can be easily styled:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

These two screen captures show the default `UISwitch` control on the left
and the customized version (setting `Appearance`) on the right in the
[Todo sample](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "Default UISwitch Color") ![](theme-images/switch-custom.png "Customized UISwitch Color")

### Other controls

Many iOS user interface controls can have their default colors and other attributes set using
the [`UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## Related Links

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Customize Tabs](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
