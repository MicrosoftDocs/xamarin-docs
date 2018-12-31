---
title: "Xamarin.Forms Map"
description: "This article explains how to use the Xamarin.Forms Map class to use the native map APIs on each platform to provide a familiar maps experience for users."
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
---

# Xamarin.Forms Map

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/WorkingWithMaps/)

_Xamarin.Forms uses the native map APIs on each platform._

Xamarin.Forms.Maps uses the native map APIs on each platform. This provides a
fast, familiar maps experience for users, but means that some configuration
steps are needed to adhere to each platforms specific API requirements.
Once configured, the `Map` control works just like any other Xamarin.Forms element in common code.

* [Maps Initialization](#Maps_Initialization) - Using `Map` requires additional
initialization code at startup.
* [Platform Configuration](#Platform_Configuration) - Each platform requires
some configuration for maps to work.
* [Using Maps in C#](#Using_Maps) - Displaying maps and pins using C#.
* [Using Maps in XAML](#Using_Xaml) - Displaying a map with XAML.

The map control has been used in the [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) sample, which is shown below.

 [![Maps in the MobileCRM sample](map-images/maps-zoom-sml.png "Map Control Example")](map-images/maps-zoom.png#lightbox "Map Control Example")

Map functionality can be further enhanced by creating a [map custom renderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## Maps Initialization

When adding maps to a Xamarin.Forms application, **Xamarin.Forms.Maps** is a
separate NuGet package that you should add to every project in the solution.
On Android, this also has a dependency on GooglePlayServices (another NuGet)
which is downloaded automatically when you add Xamarin.Forms.Maps.

After installing the NuGet package, some initialization code is required in each application project,
*after* the `Xamarin.Forms.Forms.Init` method call. For iOS use the following code:

```csharp
Xamarin.FormsMaps.Init();
```

On Android you must pass the same parameters as `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

For the Universal Windows Platform (UWP) use the following code:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Add this call in the following files for each platform:

-  **iOS** - AppDelegate.cs file, in the  `FinishedLaunching` method.
-  **Android** - MainActivity.cs file, in the  `OnCreate` method.
-  **UWP** - MainPage.xaml.cs file, in the `MainPage` constructor.

Once the NuGet package has been added and the initialization method called inside
each applcation, `Xamarin.Forms.Maps` APIs can be used in the common .NET Standard library project or Shared Project code.

<a name="Platform_Configuration" />

## Platform Configuration

Additional configuration steps are required on some platforms before the map will display.

### iOS

To access location services on iOS, you must set the following keys in **Info.plist**:

- iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – for using location services when the app is in use
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) – for using location services at all times
- iOS 10 and earlier
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – for using location services when the app is in use
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) – for using location services at all times    

To support iOS 11 and earlier, you can include all three keys: `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription`, and `NSLocationAlwaysUsageDescription`.

The XML representation for these keys in **Info.plist** is shown below. You should update the `string` values to reflect how your application is using the location information:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

The **Info.plist** entries can also be added in **Source** view while editing the **Info.plist** file:

![Info.plist for iOS 8](map-images/ios8-map-permissions.png "iOS 8 Required Info.plist Entries")


### Android

To use the [Google Maps API v2](https://developers.google.com/maps/documentation/android/)
on Android you must generate an API key and add it to your Android project.
Follow the instructions in the Xamarin doc on
[obtaining a Google Maps API v2 key](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
After following those instructions, paste the API key in the
**Properties/AndroidManifest.xml** file (view source and find/update the following element):

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

Without a valid API key the maps control will display as a grey box on Android.

> [!NOTE]
> Note that, in order for your APK to access Google Maps, you must include SHA-1 fingerprints and package names for every keystore (debug and release) that you use to sign your APK. For example, if you use one computer for debug and another computer for generating the release APK, you should include the SHA-1 certificate fingerprint from the debug keystore of the first computer and the SHA-1 certificate fingerprint from the release keystore of the second computer. Also remember to edit the key credentials if the app's **Package Name** changes. See [obtaining a Google Maps API v2 key](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

You'll also need to enable appropriate permissions by
  right-clicking on the Android project and selecting
  **Options > Build > Android Application** and ticking
  the following:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

Some of these are shown in the screenshot below:

![Required permissions for Android](map-images/android-map-permissions.png "Required Permissions for Android")

The last two are required because applications
  require a network connection to download map data. Read about Android
  [permissions](http://developer.android.com/reference/android/Manifest.permission.html)
  to learn more.

### Universal Windows Platform

To use maps on the Universal Windows Platform you must generate an authorization token. For more information, see [Request a maps authentication key](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) on MSDN.

The authentication token should then be specified in the `FormsMaps.Init("AUTHORIZATION_TOKEN")` method call, to authenticate the app with Bing Maps.

<a name="Using_Maps" />

## Using Maps

See the [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) in the MobileCRM sample for an example of how the map control can be used in code. A simple `MapPage` class might look like this - notice that a new `MapSpan` is created to position the map's view:

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### Map Type

The map content can also be changed by setting the `MapType` property, to show a regular street map (the default), satellite imagery or a combination of both.

```csharp
map.MapType == MapType.Street;
```

Valid `MapType` values are:

-  Hybrid
-  Satellite
-  Street (the default)


### Map Region and MapSpan

As shown in the code snippet above, supplying a `MapSpan` instance to a map constructor sets the initial view (center point and zoom level) of the map when it is loaded. The `MoveToRegion` method on the map class can then be used to change the position or zoom level of the map. There are two ways to create a new `MapSpan` instance:

-  **MapSpan.FromCenterAndRadius()** - static method to create a span from a  `Position` and specifying a  `Distance` .
-  **new MapSpan ()** - constructor that uses a  `Position` and the degrees of latitude and longitude to display.


To change the zoom level of the map without altering the location, create a new `MapSpan` using the current location from the `VisibleRegion.Center` property of the map control. A `Slider` could be used to control map zoom like this (however zooming directly in the map control cannot currently update the value of the slider):

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![Maps with zoom](map-images/maps-zoom-sml.png "Map Control Zoom")](map-images/maps-zoom.png#lightbox "Map Control Zoom")

### Map Pins

Locations can be marked on the map with `Pin` objects.

```csharp
var position = new Position(37,-122); // Latitude, Longitude
var pin = new Pin {
            Type = PinType.Place,
            Position = position,
            Label = "custom pin",
            Address = "custom detail info"
        };
map.Pins.Add(pin);
```

 `PinType` can be set to one of the following values, which may affect the way the pin is rendered (depending on the platform):

-  Generic
-  Place
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## Using Xaml

Maps can also be positioned in Xaml layouts as shown in this snippet.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
    x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map WidthRequest="320" HeightRequest="200"
            x:Name="MyMap"
            IsShowingUser="true"
            MapType="Hybrid"
        />
    </StackLayout>
</ContentPage>
```

The `MapRegion` and `Pins` can be set in code using the `MyMap` reference (or whatever the map is named). Note that an additional `xmlns` namespace definition is required to reference the Xamarin.Forms.Maps controls.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## Summary

The Xamarin.Forms.Maps is a separate NuGet that must be added to each project in a Xamarin.Forms solution. Additional initialization code is required, as well as some configuration steps for iOS, Android, and UWP.

Once configured the Maps API can be used to render maps with pin markers in just a few lines of code. Maps can be further enhanced with a [custom renderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## Related Links

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Map Custom Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms Samples](https://developer.xamarin.com/samples/xamarin-forms/all/)
