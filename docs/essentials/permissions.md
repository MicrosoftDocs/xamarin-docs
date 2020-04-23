---
title: "Xamarin.Essentials: Permissions"
description: "This document describes the Permissions class in Xamarin.Essentials, which provides the ability to check and request runtime permissions."
ms.assetid: 34062D84-3E55-4AF7-A688-8551068B1E57
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
---

# Xamarin.Essentials: Permissions

The **Permissions** class provides the ability to check and request runtime permissions.

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using Permissions

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

## Checking Permissions

To check the current status of a permission, use the `CheckStatusAsync` method along with the specific permission to get the status for.

```csharp
var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();
```

A `PermissionException` is thrown if the required permission is not declared.

## Requesting Permissions

To request a permission from the users, use the `RequestAsync` method along with the specific permission to request. If the user previously granted permission and has not revoked it, then this method will return `Granted` immediatelly and not display a dialog. 

```csharp
var status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();
```

A `PermissionException` is thrown if the required permission is not declared. 

Note, that on some platforms a permission request can only be activated a single time. Further prompts must be handled by the developer to check if a permision is in the `Denied` state and ask the user to manually turn it on.

## Permission Status

When using `CheckStatusAsync` or `RequestAsync` a `PermissionStatus` will be returned that be used to determine the next steps.

* Unknown - The permission is in an unknown state
* Denied - The user denied the permission request
* Disabled - The feature is disabled on the device
* Granted - The user granted permission or is automatically granted
* Restricted - In a restricted state

## Available Permissions

Xamarin.Essentials attempts to abstract as many permissions as possible, however each operating system has a different set of runtime permissions. Additionally, there are differences in being able to provide a single API for some permissions. Here is a guide to the currently available permissions:

Icon Guide:

* ![Full Support](~/media/shared/yes.png "Full Support") - Supported
* ![Not Supported](~/media/shared/no.png "Not supported or required") - Not supported/required

| Permission | Android | iOS | UWP | watchOS | tvOS | Tizen |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: 
| CalendarRead   | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS supported](~/media/shared/yes.png "watchOS supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| CalendarWrite | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS supported](~/media/shared/yes.png "watchOS supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| Camera | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen  supported](~/media/shared/yes.png "Tizen supported") |
| ContactsRead | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP supported](~/media/shared/yes.png "UWP supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| ContactsWrite | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP supported](~/media/shared/yes.png "UWP supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| Flashlight | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS not supported](~/media/shared/no.png "iOS not supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen  supported](~/media/shared/yes.png "Tizen supported") |
| LocationWhenInUse | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP supported](~/media/shared/yes.png "UWP supported") | ![watchOS supported](~/media/shared/yes.png "watchOS supported") | ![tvOS supported](~/media/shared/yes.png "tvOS supported")  | ![Tizen  supported](~/media/shared/yes.png "Tizen supported") |
| LocationAlways | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP supported](~/media/shared/yes.png "UWP supported") | ![watchOS supported](~/media/shared/yes.png "watchOS supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen  supported](~/media/shared/yes.png "Tizen supported") |
| Media | ![Android not supported](~/media/shared/no.png "Android not supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| Microphone | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP supported](~/media/shared/yes.png "UWP supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen  supported](~/media/shared/yes.png "Tizen supported") |
| Phone | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| Photos | ![Android not supported](~/media/shared/no.png "Android not supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS supported](~/media/shared/yes.png "tvOS supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| Reminders | ![Android not supported](~/media/shared/no.png "Android not supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS supported](~/media/shared/yes.png "watchOS supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| Sensors | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP supported](~/media/shared/yes.png "UWP supported") | ![watchOS supported](~/media/shared/yes.png "watchOS supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| Sms | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| Speech | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS supported](~/media/shared/yes.png "iOS supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| StorageRead | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS not supported](~/media/shared/no.png "iOS not supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |
| StorageWrite | ![Android supported](~/media/shared/yes.png "Android supported") | ![iOS not supported](~/media/shared/no.png "iOS not supported") | ![UWP not supported](~/media/shared/no.png "UWP not supported") | ![watchOS not supported](~/media/shared/no.png "watchOS not supported") | ![tvOS not supported](~/media/shared/no.png "tvOS not supported") | ![Tizen not supported](~/media/shared/no.png "Tizen not supported") |

If a permission is marked as ![not supported](~/media/shared/no.png "not supported") it will always return `Granted` when checked or requested.

## General Usage
Here is a general usage pattern for handling permissions.

```csharp
public async Task<PermissionStatus> CheckAndRequestLocationPermission()
{
    var status = await Permissions.CheckStatusAsync<Permissions.LocationWhenInUse>();
    if (status != PermissionStatus.Granted)
    {
        status = await Permissions.RequestAsync<Permissions.LocationWhenInUse>();
    }

    // Additionally could prompt the user to turn on in settings

    return status;
}
```

Each permission type can have an instance of it created that the methods can be called directly.

```csharp
public async Task GetLocationAsync()
{
    var status = await CheckAndRequestPermissionAsync(new Permissions.LocationWhenInUse());
    if (status != PermissionStatus.Granted)
    {
        // Notify user permission was denied
        return;
    }

    var location = await Geolocation.GetLocationAsync();
}

public async Task<PermissionStatus> CheckAndRequestPermissionAsync<T>(T permission)
            where T : BasePermission
{
    var status = await permission.CheckStatusAsync();
    if (status != PermissionStatus.Granted)
    {
        status = await permission.RequestAsync();
    }

    return status;
}
```

## Extending Permissions

The Permissions API was created to be flexible and extensible for applications that require additional validation or permissions that aren't included in Xamarin.Essentials. Create a new class that inherits from `BasePermission` and implement the required abstract methods. Then 

```csharp
public class MyPermission : BasePermission
{
    // This method checks if current status of the permission
    public override Task<PermissionStatus> CheckStatusAsync()
    {
        throw new System.NotImplementedException();
    }

    // This method is optional and a PermissionException is often thrown if a permission is not declared
    public override void EnsureDeclared()
    {
        throw new System.NotImplementedException();
    }

    // Requests the user to accept or deny a permission
    public override Task<PermissionStatus> RequestAsync()
    {
        throw new System.NotImplementedException();
    }
}
```

When implementing a permission in a specific platform, the `BasePlatformPermission` class can be inherited from. This provides additional platform helpers methods to automatically check the declarations.

## Platform Implementation Specifics

# [Android](#tab/android)

Permissions must have the matching attributes set in the Android Manifest file.

Read more on the [Permissions in Xamarin.Android](https://docs.microsoft.com/xamarin/android/app-fundamentals/permissions) documentation.

# [iOS](#tab/ios)

Permissions must have a matching string in the `Info.plist` file. Once a permission is requested and denied a pop up will no longer appear if you request the permission a second time. You must prompt your user to manually adjust the setting in the applications settings screen in iOS.

Read more on the [iOS Security and Privacy Features](https://docs.microsoft.com/xamarin/ios/app-fundamentals/security-privacy) documentation.

# [UWP](#tab/uwp)

Permissions must have matching capabilities declared in the package manifest.

Read more on the [App Capability Declaration](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) documentation.

--------------

## API

- [Permissions source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Permissions)
- [Permissions API documentation](xref:Xamarin.Essentials.Permissions)

