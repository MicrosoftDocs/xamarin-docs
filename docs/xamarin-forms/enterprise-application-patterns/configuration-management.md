---
title: "Configuration Management"
description: "This chapter explains how the eShopOnContainers mobile app implements configuration management to provide app settings and user settings."
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Configuration Management

> [!NOTE]
> This eBook was published in the spring of 2017, and has not been updated since then. There is much in the book that remains valuable, but some of the material is outdated.

Settings allow the separation of data that configures the behavior of an app from the code, allowing the behavior to be changed without rebuilding the app. There are two types of settings: app settings, and user settings.

App settings are data that an app creates and manages. It can include data such as fixed web service endpoints, API keys, and runtime state. App settings are tied to the existence of the app and are only meaningful to that app.

User settings are the customizable settings of an app that affect the behavior of the app and don't require frequent re-adjustment. For example, an app might let the user specify where to retrieve data from, and how to display it on the screen.

Xamarin.Forms includes a persistent dictionary that can be used to store settings data. This dictionary can be accessed using the [`Application.Current.Properties`](xref:Xamarin.Forms.Application.Properties) property, and any data that's placed into it is saved when the app goes into a sleep state, and is restored when the app resumes or starts up again. In addition, the [`Application`](xref:Xamarin.Forms.Application) class also has a [`SavePropertiesAsync`](xref:Xamarin.Forms.Application.SavePropertiesAsync) method that allows an app to have its settings saved when required. For more information about this dictionary, see [Properties Dictionary](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary).

A downside to storing data using the Xamarin.Forms persistent dictionary is that it's not easily data bound to. Therefore, the eShopOnContainers mobile app uses the Xam.Plugins.Settings library, available from [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). This library provides a consistent, type-safe, cross-platform approach for persisting and retrieving app and user settings, while using the native settings management provided by each platform. In addition, it's straightforward to use data binding to access settings data exposed by the library.

> [!NOTE]
> While the Xam.Plugin.Settings library can store both app and user settings, it makes no distinction between the two.

## Creating a Settings Class

When using the Xam.Plugins.Settings library, a single static class should be created that will contain the app and user settings required by the app. The following code example shows the Settings class in the eShopOnContainers mobile app:

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

Settings can be read and written through the `ISettings` API, which is provided by the Xam.Plugins.Settings library. This library provides a singleton that can be used to access the API, `CrossSettings.Current`, and an app's settings class should expose this singleton via an `ISettings` property.

> [!NOTE]
> Using directives for the Plugin.Settings and Plugin.Settings.Abstractions namespaces should be added to a class that requires access to the Xam.Plugins.Settings library types.

## Adding a Setting

Each setting consists of a key, a default value, and a property. The following code example shows all three items for a user setting that represents the base URL for the online services that the eShopOnContainers mobile app connects to:

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

The key is always a const string that defines the key name, with the default value for the setting being a static readonly value of the required type. Providing a default value ensures that a valid value is available if an unset setting is retrieved.

The `UrlBase` static property uses two methods from the `ISettings` API to read or write the setting value. The `ISettings.GetValueOrDefault` method is used to retrieve a setting's value from platform-specific storage. If no value is defined for the setting, its default value is retrieved instead. Similarly, the `ISettings.AddOrUpdateValue` method is used to persist a setting's value to platform-specific storage.

Rather that define a default value inside the `Settings` class, the `UrlBaseDefault` string obtains its value from the `GlobalSetting` class. The following code example shows the `BaseEndpoint` property and `UpdateEndpoint` method in this class:

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

Each time the `BaseEndpoint` property is set, the `UpdateEndpoint` method is called. This method updates a series of properties, all of which are based on the `UrlBase` user setting that's provided by the `Settings` class that represent different endpoints that the eShopOnContainers mobile app connects to.

## Data Binding to User Settings

In the eShopOnContainers mobile app, the `SettingsView` exposes two user settings. These settings allow configuration of whether the app should retrieve data from microservices that are deployed as Docker containers, or whether the app should retrieve data from mock services that don't require an internet connection. When choosing to retrieve data from containerized microservices, a base endpoint URL for the microservices must be specified. Figure 7-1 shows the `SettingsView` when the user has chosen to retrieve data from containerized microservices.

![User settings exposed by the eShopOnContainers mobile app](configuration-management-images/settings-endpoint.png)

**Figure 7-1**: User settings exposed by the eShopOnContainers mobile app

Data binding can be used to retrieve and set settings exposed by the `Settings` class. This is achieved by controls on the view binding to view model properties that in turn access properties in the `Settings` class, and raising a property changed notification if the settings value has changed. For information about how the eShopOnContainers mobile app constructs view models and associates them to views, see [Automatically Creating a View Model with a View Model Locator](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically-creating-a-view-model-with-a-view-model-locator).

The following code example shows the [`Entry`](xref:Xamarin.Forms.Entry) control from the `SettingsView` that allows the user to enter a base endpoint URL for the containerized microservices:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

This [`Entry`](xref:Xamarin.Forms.Entry) control binds to the `Endpoint` property of the `SettingsViewModel` class, using a two-way binding. The following code example shows the Endpoint property:

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

When the `Endpoint` property is set the `UpdateEndpoint` method is called, provided that the supplied value is valid, and property changed notification is raised. The following code example shows the `UpdateEndpoint` method:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

This method updates the `UrlBase` property in the `Settings` class with the base endpoint URL value entered by the user, which causes it to be persisted to platform-specific storage.

When the `SettingsView` is navigated to, the `InitializeAsync` method in the `SettingsViewModel` class is executed. The following code example shows this method:

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

The method sets the `Endpoint` property to the value of the `UrlBase` property in the `Settings` class. Accessing the `UrlBase` property causes the Xam.Plugins.Settings library to retrieve the settings value from platform-specific storage. For information about how the `InitializeAsync` method is invoked, see [Passing Parameters During Navigation](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing-parameters-during-navigation).

This mechanism ensures that whenever a user navigates to the SettingsView, user settings are retrieved from platform-specific storage and presented through data binding. Then, if the user changes the settings values, data binding ensures that they are immediately persisted back to platform-specific storage.

## Summary

Settings allow the separation of data that configures the behavior of an app from the code, allowing the behavior to be changed without rebuilding the app. App settings are data that an app creates and manages, and user settings are the customizable settings of an app that affect the behavior of the app and don't require frequent re-adjustment.

The Xam.Plugins.Settings library provides a consistent, type-safe, cross-platform approach for persisting and retrieving app and user settings, and data binding can be used to access settings created with the library.

## Related Links

- [Download eBook (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
