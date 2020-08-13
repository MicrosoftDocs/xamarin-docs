---
title: "Part 4 - Dealing with Multiple Platforms"
description: "This document describes how to handle application divergence based on platform or capability. It discusses screen size, navigation metaphors, touch and gestures, push notifications, and interface paradigms such as lists and tabs."
ms.prod: xamarin
ms.assetid: BBE47BA8-78BC-6A2B-63BA-D1A45CB1D3A5
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
---

# Part 4 - Dealing with Multiple Platforms

## Handling Platform Divergence &amp; Features

Divergence isn’t just a ‘cross-platform’ problem; devices on the
‘same’ platform have different capabilities (especially the wide variety of
Android devices that are available). The most obvious and basic is screen size,
but other device attributes can vary and require an application to check for
certain capabilities and behave differently based on their presence (or
absence).

This means all applications need to deal with graceful degradation of
functionality, or else present an unattractive, lowest-common-denominator
feature set. Xamarin’s deep integration with the native SDKs of each platform
allow applications to take advantage of platform-specific functionality, so it
makes sense to design apps to use those features.

See the Platform Capabilities documentation for an overview of how the
platforms differ in functionality.

## Examples of Platform Divergence

### Fundamental elements that exist across platforms

There are some characteristics of mobile applications that are universal.
These are higher-level concepts that are generally true of all devices and can
therefore form the basis of your application’s design:

- Feature selection via tabs or menus
- Lists of data and scrolling
- Single views of data
- Editing single views of data
- Navigating back

When designing your high-level screen flow you can base a common user
experience on these concepts.

### Platform-specific attributes

In addition to the basic elements that exist on all platforms, you will need
to address key platform differences in your design. You may need to consider
(and write code specifically to handle) these differences:

- **Screen sizes** – Some platforms (like iOS and earlier Windows Phone versions) have standardized screen sizes that are relatively simple to target. Android devices have a large variety of screen dimensions, which require more effort to support in your application.
- **Navigation metaphors** – Differ across platforms (eg. hardware ‘back’ button, Panorama UI control) and within platforms (Android 2 and 4, iPhone vs iPad).
- **Keyboards** – Some Android devices have physical keyboards while others only have a software keyboard. Code that detects when a soft-keyboard is obscuring part of the screen needs to be sensitive to these differences.
- **Touch and gestures** – Operating system support for gesture recognition varies, especially in older versions of each operating system. Earlier versions of Android have very limited support for touch operations, meaning that supporting older devices may require separate code
- **Push notifications** – There are different capabilities/implementations on each platform (eg. Live Tiles on Windows).

### Device-specific features

Determine what the minimum features required for the application must be; or
when decide what additional features to take advantage of on each platform. Code
will be required to detect features and disable functionality or offer
alternatives (eg. an alternative to geo-location could be to let the user type a
location or choose from a map):

- **Camera** – Functionality differs across devices: some devices don’t have a camera, others have both front- and rear-facing cameras. Some cameras are capable of video recording.
- **Geo-location & maps** – Support for GPS or Wi-Fi location is not present on all devices. Apps also need to cater for the varying levels of accuracy that’s supported by each method.
- **Accelerometer, gyroscope and compass** – These features are often found in only a selection of devices on each platform, so apps almost always need to provide a fallback when the hardware isn’t supported.
- **Twitter and Facebook** – only ‘built-in’ on iOS5 and iOS6 respectively. On earlier versions and other platforms you will need to provide your own authentication functions and interface directly with each services’ API.
- **Near Field Communications (NFC)** – Only on (some) Android phones (at time of writing).

## Dealing with Platform Divergence

There are two different approaches to supporting multiple platforms from the
same code-base, each with its own set of benefits and disadvantages.

- **Platform Abstraction** – Business Façade pattern, provides a unified access across platforms and abstracts the particular platform implementations into a single, unified API.
- **Divergent Implementation** – Invocation of specific platform features via divergent implementations via architectural tools such as interfaces and inheritance or conditional compilation.

## Platform Abstraction

### Class Abstraction

Using either interfaces or base classes defined in the shared code and
implemented or extended in platform-specific projects. Writing and extending
shared code with class abstractions is particularly suited to Portable Class
Libraries because they have a limited subset of the framework available to them
and cannot contain compiler directives to support platform-specific code
branches.

#### Interfaces

Using interfaces allows you to implement platform-specific classes that can
still be passed into your shared libraries to take advantage of common code.

The interface is defined in the shared code, and passed into the shared
library as a parameter or property.

The platform-specific applications can then implement the interface and still
take advantage of shared code to ‘process’ it.

 **Advantages**

The implementation can contain platform-specific code and even reference
platform-specific external libraries.

 **Disadvantages**

Having to create and pass implementations into the shared code. If the
interface is used deep within the shared code then it ends up being passed
through multiple method parameters or otherwise pushed down through the call
chain. If the shared code uses lots of different interfaces then they must all
be created and set in the shared code somewhere.

#### Inheritance

The shared code could implement abstract or virtual classes that could be
extended in one or more platform-specific projects. This is similar to using
interfaces but with some behavior already implemented. There are different
viewpoints on whether interfaces or inheritance are a better design choice: in
particular because C# only allows single inheritance it can dictate the way your
APIs can be designed going forward. Use inheritance with caution.

The advantages and disadvantages of interfaces apply equally to inheritance,
with the additional advantage that the base class can contain some
implementation code (perhaps an entire platform agnostic implementation that can
be optionally extended).

## Xamarin.Forms

See the [Xamarin.Forms](~/get-started/index.yml) documentation.

### Other Cross-Platform Libraries

These libraries also offer cross-platform functionality for C# developers:

- [**Xamarin.Essentials**](~/essentials/index.md) – Cross-platform APIs for common features.
- [**SkiaSharp**](~/xamarin-forms/user-interface/graphics/skiasharp/index.md) – Cross-platform 2D graphics.

## Conditional Compilation

There are some situations where your shared code will still need to work
differently on each platform, possibly accessing classes or features that behave
differently. Conditional compilation works best with Shared Asset Projects,
where the same source file is
being referenced in multiple projects that have different symbols defined.

Xamarin projects always define `__MOBILE__` which is true for both iOS and
Android application projects (note the double-underscore pre- and post-fix on
these symbols).

```csharp
#if __MOBILE__
// Xamarin iOS or Android-specific code
#endif
```

#### iOS

Xamarin.iOS defines `__IOS__` which you can use to detect iOS devices.

```csharp
#if __IOS__
// iOS-specific code
#endif
```

There are also Watch- and TV-specific symbols:

```csharp
#if __TVOS__
// tv-specific stuff
#endif

#if __WATCHOS__
// watch-specific stuff
#endif
```

#### Android

Code that should only be compiled into Xamarin.Android applications can use
the following

```csharp
#if __ANDROID__
// Android-specific code
#endif
```

Each API version also defines a new compiler directive, so code like this
will let you add features if newer APIs are targeted. Each API level includes
all the ‘lower’ level symbols. This feature is not really useful for
supporting multiple platforms; typically the `__ANDROID__` symbol
will be sufficient.

```csharp
#if __ANDROID_11__
// code that should only run on Android 3.0 Honeycomb or newer
#endif
```

#### Mac

Xamarin.Mac defines `__MACOS__` which you can use to compile for macOS only:

```csharp
#if __MACOS__
// macOS-specific code
#endif
```

#### Universal Windows Platform (UWP)

Use `WINDOWS_UWP`. There are no underscores surrounding the string like the Xamarin
platform symbols.

```csharp
#if WINDOWS_UWP
// UWP-specific code
#endif
```

#### Using Conditional Compilation

A simple case-study example of conditional compilation is setting the file
location for the SQLite database file. The three platforms have slightly
different requirements for specifying the file location:

- **iOS** – Apple prefers non-user data to be placed in a specific location (the Library directory), but there is no system constant for this directory. Platform-specific code is required to build the correct path.
- **Android** – The system path returned by  `Environment.SpecialFolder.Personal` is an acceptable location to store the database file.
- **Windows Phone** – The isolated storage mechanism does not allow a full path to be specified, just a relative path and filename.
- **Universal Windows Platform** – Uses `Windows.Storage` APIs.

The following code uses conditional compilation to ensure the `DatabaseFilePath` is correct for each platform:

```csharp
public static string DatabaseFilePath
{
    get
    {
        var filename = "TodoDatabase.db3";
#if SILVERLIGHT
        // Windows Phone 8
        var path = filename;
#else

#if __ANDROID__
        string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal);
#else
#if __IOS__
        // we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library");
#else
        // UWP
        string libraryPath = Windows.Storage.ApplicationData.Current.LocalFolder.Path;
#endif
#endif
        var path = Path.Combine(libraryPath, filename);
#endif
        return path;
    }
}
```

The result is a class that can be built and used on all platforms,
placing the SQLite database file in a different location on each platform.
