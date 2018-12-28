---
title: "Creating a Service"
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
---

# Creating a Service

Xamarin.Android services must obey two inviolable rules of Android services:

* They must extend the [`Android.App.Service`](https://developer.xamarin.com/api/type/Android.App.Service/).
* They must be decorated with the [`Android.App.ServiceAttribute`](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/).

Another requirement of Android services is that they must be registered in the **AndroidManifest.xml** and given a unique name. Xamarin.Android will automatically register the service in the manifest at build time with the necessary XML attribute.

This code snippet is the simplest example of creating a service in Xamarin.Android that meets these two requirements:  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

At compile time, Xamarin.Android will register the service by injecting the following XML element into **AndroidManifest.xml**  (notice that Xamarin.Android generated a random name for the service):

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

It is possible to share a service with other Android applications by _exporting_ it. This is accomplished by setting the `Exported` property on the `ServiceAttribute`. When exporting a service, the `ServiceAttribute.Name` property should also be set to provide a meaningful public name for the service. This snippet demonstrates how to export and name a service:

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

The **AndroidManifest.xml** element for this service will then look something like:

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

Services have their own lifecycle with callback methods that are invoked as the service is created. Exactly which methods are invoked depends on the type of service. A started service must implement different lifecycle methods than a bound service, while a hybrid service must implement the callback methods for both a started service and a bound service. These methods are all members of the `Service` class; how the service is started will determine what lifecycle methods will be invoked. These lifecycle methods will be discussed in more detail later.

By default, a service will start in the same process as an Android application. It is possible to start a service in its own process by setting the `ServiceAttribute.IsolatedProcess` property to true:

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

The next step is to examine how to start a service and then move on to examine how to implement the three different types of services.

> [!NOTE]
> A service runs on the UI thread, so if any work is to be performed which blocks the UI, the service must use threads to perform the work.

## Starting A Service

The most basic way to start a service in Android is to dispatch an `Intent` which contains meta-data to help identify which service should be started. There are two different styles of Intents that can be used to start a service:

-   **Explicit Intent** &ndash; An _explicit Intent_ will identify
    exactly what service should be used to complete a given action. An
    explicit Intent can be thought of as a letter that has a specific
    address; Android will route the intent to the service that is
    explicitly identified. This snippet is one example of using an
    explicit Intent to start a service called `DownloadService`:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

-   **Implicit Intent** &ndash; This type of Intent loosely identifies
    the of action that the user wishes to perform, but the exact service to
    complete that action is unknown. An implicit Intent can be thought
    of as a letter that is addressed "To Whom It May Concern...".
    Android will examine the contents of the Intent, and determine if
    there is an existing service which matches the intent.

    An _intent filter_ is used to help match the implicit intent with a
    registered service. An intent filter is an XML element that is
    added to **AndroidManifest.xml** which contains the necessary
    meta-data to help match a Service with an implicit intent.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

If Android has more than one possible match for an implicit intent,
then it may ask the user to select the component to handle the
action:

![Screenshot of a disambiguation dialog](images/creating-a-service-01.png "Screenshot of a disambiguation dialog")

> [!IMPORTANT]
> Starting in Android 5.0 (AP level 21) an implicit intent cannot be used to start a service.

Where possible, applications should use explicit Intents to start a service. An implicit Intent does not ask for a specific service to start &ndash; it is a request for some service installed on the device to handle the request. This ambiguous request can result in the wrong service handling the request or another app needlessly starting (which increases the pressure for resources on the device).

How the Intent is dispatched depends on the type of service and will be discussed in more detail later in the guides specific to each type of service.


### Creating an Intent Filter for Implicit Intents

To associate a service with an implicit Intent, an Android app must provide some meta-data to identify the capabilities of the service. This meta-data is provided by  _intent filters_. Intent filters contain some information, such as an action or a type of data, that must be present in an Intent to start a service. In Xamarin.Android, the intent filter is registered in **AndroidManifest.xml** by decorating a service with the [`IntentFilterAttribute`](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/). For example, the following code adds an intent filter with an associated action of `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

This results in an entry being included in the **AndroidManifest.xml** file &ndash; an entry that is packaged with the application in a way analogous to the following example:

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

With the basics of a Xamarin.Android service out of the way, let's examine the different subtypes of services in more detail.


## Related Links

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.ServiceAttribute](https://developer.xamarin.com/api/type/Android.App.ServiceAttribute/)
- [Android.App.Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [Android.App.IntentFilterAttribute](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
