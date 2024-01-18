---
title: "Connect to Local Web Services from iOS Simulators and Android Emulators"
description: "This article explains how a Xamarin mobile application running in the iOS simulator or Android emulator can consume a ASP.NET Core web service running locally."
ms.service: xamarin
ms.assetid: FD8FE199-898B-4841-8041-CC9CA1A00917
author: davidbritch
ms.author: dabritch
ms.date: 02/04/2021
no-loc: [Objective-C]
---

# Connect to local web services from iOS simulators and Android emulators

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/webservices-todorest/)

Many mobile applications consume web services. During the development phase, it's common to deploy a web service locally and consume it from a mobile application running in the iOS simulator or Android emulator. This avoids having to deploy the web service to a hosted endpoint, and enables a straightforward debugging experience because both the mobile application and web service are running locally.

Mobile applications running in the iOS simulator or Android emulator can consume ASP.NET Core web services that are running locally, and exposed over HTTP, as follows:

- Applications running in the iOS simulator can connect to local HTTP web services via your machines IP address, or via the `localhost` hostname. For example, given a local HTTP web service that exposes a GET operation via the `/api/todoitems/` relative URI, an application running in the iOS simulator can consume the operation by sending a GET request to `http://localhost:<port>/api/todoitems/`.
- Applications running in the Android emulator can connect to local HTTP web services via the `10.0.2.2` address, which is an alias to your host loopback interface (`127.0.0.1` on your development machine). For example, given a local HTTP web service that exposes a GET operation via the `/api/todoitems/` relative URI, an application running in the Android emulator can consume the operation by sending a GET request to `http://10.0.2.2:<port>/api/todoitems/`.

However, additional work is necessary for an application running in the iOS simulator or Android emulator to consume a local web service that is exposed over HTTPS. For this scenario, the process is as follows:

1. Create a self-signed development certificate on your machine. For more information, see [Create a development certificate](#create-a-development-certificate).
1. Configure your project to use the appropriate `HttpClient` network stack for your debug build. For more information, see [Configure your project](#configure-your-project).
1. Specify the address of your local machine. For more information, see [Specify the local machine address](#specify-the-local-machine-address).
1. Bypass the local development certificate security check. For more information, see [Bypass the certificate security check](#bypass-the-certificate-security-check).

Each item will be discussed in turn.

## Create a development certificate

Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store. However, while the certificate has been installed, it's not trusted. To trust the certificate, perform the following one-time step to run the dotnet `dev-certs` tool:

```dotnetcli
dotnet dev-certs https --trust
```

The following command provides help on the `dev-certs` tool:

```dotnetcli
dotnet dev-certs https --help
```

Alternatively, when you run an ASP.NET Core 2.1 project (or above), that uses HTTPS, Visual Studio will detect if the development certificate is missing and will offer to install it and trust it.

> [!NOTE]
> The ASP.NET Core HTTPS development certificate is self-signed.

For more information about enabling local HTTPS on your machine, see [Enable local HTTPS](/aspnet/core/getting-started#enable-local-https).

## Configure your project

Xamarin applications running on iOS and Android can specify which networking stack is used by the `HttpClient` class, with the choices being a managed network stack, or native network stacks. The managed stack provides a high level of compatibility with existing .NET code, but is limited to TLS 1.0 and can be slower and result in a larger executable size. The native stacks can be faster and provide better security, but may not provide all the functionality of the `HttpClient` class.

### iOS

Xamarin applications running on iOS can use the managed network stack, or the native `CFNetwork` or `NSUrlSession` network stacks. By default, new iOS platform projects use the `NSUrlSession` network stack, to support TLS 1.2, and use native APIs for better performance and smaller executable size. For more information, see [HttpClient and SSL/TLS implementation selector for iOS/macOS](~/cross-platform/macios/http-stack.md).

### Android

Xamarin applications running on Android can use the managed `HttpClient` network stack, or the native `AndroidClientHandler` network stack. By default, new Android platform projects use the `AndroidClientHandler` network stack, to support TLS 1.2, and use native APIs for better performance and smaller executable size. For more information about Android network stacks, see [HttpClient Stack and SSL/TLS Implementation selector for Android](~/android/app-fundamentals/http-stack.md).

## Specify the local machine address

The iOS simulator and Android emulator both provide access to secure web services running on your local machine. However, the local machine address is different for each.

### iOS

The iOS simulator uses the host machine network. Therefore, applications running in the simulator can connect to web services running on your local machine via the machines IP address or via the `localhost` hostname. For example, given a local secure web service that exposes a GET operation via the `/api/todoitems/` relative URI, an application running on the iOS simulator can consume the operation by sending a GET request to `https://localhost:<port>/api/todoitems/`.

> [!NOTE]
> When running a mobile application in the iOS simulator from Windows, the application is displayed in the [remoted iOS simulator for Windows](~/tools/ios-simulator/index.md). However, the application is running on the paired Mac. Therefore, there's no localhost access to a web service running in Windows for an iOS application running on a Mac.

### Android

Each instance of the Android emulator is isolated from your development machine network interfaces, and runs behind a virtual router. Therefore, an emulated device can't see your development machine or other emulator instances on the network.

However, the virtual router for each emulator manages a special network space that includes pre-allocated addresses, with the `10.0.2.2` address being an alias to your host loopback interface (127.0.0.1 on your development machine). Therefore, given a local secure web service that exposes a GET operation via the `/api/todoitems/` relative URI, an application running on the Android emulator can consume the operation by sending a GET request to `https://10.0.2.2:<port>/api/todoitems/`.

### Detect the operating system

The [`DeviceInfo`](xref:Xamarin.Essentials.DeviceInfo) class can be used to detect the platform the application is running on. The appropriate hostname, that enables access to local secure web services, can then be set as follows:

```csharp
public static string BaseAddress =
    DeviceInfo.Platform == DevicePlatform.Android ? "https://10.0.2.2:5001" : "https://localhost:5001";
public static string TodoItemsUrl = $"{BaseAddress}/api/todoitems/";
```

For more information about the `DeviceInfo` class, see [Xamarin.Essentials: Device Information](~/essentials/device-information.md).

## Bypass the certificate security check

Attempting to invoke a local secure web service from an application running in the iOS simulator or Android emulator will result in a `HttpRequestException` being thrown, even when using the managed network stack on each platform. This is because the local HTTPS development certificate is self-signed, and self-signed certificates aren't trusted by iOS or Android. Therefore, it's necessary to ignore SSL errors when an application consumes a local secure web service. This can be accomplished when using both the managed and native network stacks on iOS and Android, by setting the `ServerCertificateCustomValidationCallback` property on a `HttpClientHandler` object to a callback that ignores the result of the certificate security check for the local HTTPS development certificate:

```csharp
// This method must be in a class in a platform project, even if
// the HttpClient object is constructed in a shared project.
public HttpClientHandler GetInsecureHandler()
{
    HttpClientHandler handler = new HttpClientHandler();
    handler.ServerCertificateCustomValidationCallback = (message, cert, chain, errors) =>
    {
        if (cert.Issuer.Equals("CN=localhost"))
            return true;
        return errors == System.Net.Security.SslPolicyErrors.None;
    };
    return handler;
}
```

In this code example, the server certificate validation result is returned when the certificate that underwent validation is not the `localhost` certificate. For this certificate, the validation result is ignored and `true` is returned, indicating that the certificate is valid. The resulting `HttpClientHandler` object should be passed as an argument to the `HttpClient` constructor for debug builds:

```csharp
#if DEBUG
    HttpClientHandler insecureHandler = GetInsecureHandler();
    HttpClient client = new HttpClient(insecureHandler);
#else
    HttpClient client = new HttpClient();
#endif
```

## Enable HTTP clear-text traffic

Optionally, you can configure your iOS and Android projects to allow clear-text HTTP traffic. If the backend service is configured to allow HTTP traffic you can specify HTTP in the base URLs and then configure your projects to allow clear-text traffic:

```csharp
public static string BaseAddress =
    DeviceInfo.Platform == DevicePlatform.Android ? "http://10.0.2.2:5000" : "http://localhost:5000";
public static string TodoItemsUrl = $"{BaseAddress}/api/todoitems/";
```

### iOS ATS opt-out

To enable clear-text local traffic on iOS you should [opt-out of ATS](~/ios/app-fundamentals/ats.md#optout) by adding the following to your **Info.plist** file:

```xml
<key>NSAppTransportSecurity</key>    
<dict>
    <key>NSAllowsLocalNetworking</key>
    <true/>
</dict>
```

### Android network security configuration

To enable clear-text local traffic on Android you should create a network security configuration by adding a new XML file named **network_security_config.xml** in the **Resources/xml** folder. The XML file should specify the following configuration:

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
  <domain-config cleartextTrafficPermitted="true">
    <domain includeSubdomains="true">10.0.2.2</domain>
  </domain-config>
</network-security-config>
```

Then, configure the **networkSecurityConfig** property on the **application** node in the Android Manifest:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest>
    <application android:networkSecurityConfig="@xml/network_security_config">
        ...
    </application>
</manifest>
```
Make sure the Build Action is set as AndroidResource, otherwise the XML file will not be found at build time.

## Related links

- [TodoREST (sample)](/samples/xamarin/xamarin-forms-samples/webservices-todorest/)
- [Enable local HTTPS](/aspnet/core/getting-started#enable-local-https)
- [HttpClient and SSL/TLS implementation selector for iOS/macOS](~/cross-platform/macios/http-stack.md)
- [HttpClient Stack and SSL/TLS Implementation selector for Android](~/android/app-fundamentals/http-stack.md)
- [Android Network Security Configuration](https://devblogs.microsoft.com/xamarin/cleartext-http-android-network-security/)
- [iOS App Transport Security](~/ios/app-fundamentals/ats.md)
- [Xamarin.Essentials: Device Information](~/essentials/device-information.md)
