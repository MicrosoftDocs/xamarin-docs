---
title: "Connect to Local Web Services from iOS Simulators and Android Emulators"
description: "This article explains how a Xamarin mobile application running in the iOS simulator or Android emulator can consume a ASP.NET Core web service running locally."
ms.prod: xamarin
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

### iOS

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

### Android

#### Generate the Customized Self-Signed TLS Development Certificate

Normally the HTTPS development certificate for an ASP.NET Core Web API can be generated using the .NET Core tools, with a simple call to:

`dotnet dev-certs https`

However, since we have an Android application that needs to reach from the emulator to the ASP.NET Core API, we need to make sure that the Android emulator's networking space is allowed to authenticate against the API. The Android emulator creates a `10.0.2.2` network address as an alias for `127.0.0.1` or `localhost`. Because of this alias, the default SSL certificate generated for ASP.NET Core Web API will not work.

We need to generate the self-signed certificate with a Subject Alternative Name (SAN) entry for the IP Address `10.0.2.2`. To do this, we use the [New-SelfSignedCertificate](https://docs.microsoft.com/en-us/powershell/module/pki/new-selfsignedcertificate?view=windowsserver2019-ps) PowerShell command:
```
New-SelfSignedCertificate -Type SSLServerAuthentication -Subject "localhost" -KeyUsage DigitalSignature,KeyEncipherment -CertStoreLocation "cert:\CurrentUser\My" -FriendlyName "Android and ASP.NET HTTPS Self-signed Certificate" -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1", "2.5.29.19={text}", "1.3.6.1.4.1.311.84.1.1=02", "2.5.29.17={text}DNS=localhost&IPAddress=10.0.2.2")
```

Explanation of what is happening with each parameter within this command.

- **Type**
  - The Type is set to SSLServerAuthentication, because this the purpose of the certificate.

- **Subject**
  - The Subject is set to `localhost` so that the `Subject` and `Issuer` will be set to `localhost` in the certificate that is generated.

- **KeyUsage**
  - The `DigitalSignature` and `KeyEncipherment` values allow the certificate to be used for signing messages during HTTPS communications.

- **CertStoreLocation**
  - This tells the command to place the generated certificate into the `Personal > Certificates` store of the current user. It is better to place this development certificate in the CurrentUser store, so that the certificate is not trusted across the entire LocalMachine.

- **FriendlyName**
  - The friendly name we want the generated certificate to have. This can be practically any string value.

- **TextExtension**
  - This allows us to modify the certificate's properties using an array of Object Identifiers (OID's). Each OID has a specific purpose.

    - **2.5.29.37:** Enhanced Key Usage (EKU)
      - **1.3.6.1.5.5.7.3.1:** Server Authentication
    - **2.5.29.19:** Basic Constraints
    - **1.3.6.1.4.1.311.84.1.1:** A Microsoft-specific OID that ensures that .NET Core tooling identifies the certificate as valid for use in HTTPS message signing. Because of the way in which the source code for the `dotnet dev-certs --check` command validates certificates, this OID must also have a raw byte value of at least 2. Please see [this StackOverflow answer](https://stackoverflow.com/a/65958649/13159009) and the [dotnet dev-certs source code](https://github.com/dotnet/aspnetcore/blob/c4d043dc86e52e8b83f8c0ef3500f2985f90c23c/src/Shared/CertificateGeneration/CertificateManager.cs#L83) for more information about how the .NET Core tooling validates self-signed HTTPS/TLS certificates.

    - **2.5.29.17:** Subject Alternative Name (SAN)
      - While it is tempting to use the `-DnsName` parameter of the PowerShell command, in the case of the Android development certificate, we need to define the SAN using the `TextExtension` parameter, because the `-DnsName` parameter will not properly append the `10.0.2.2` IP address as part of the certificate's SAN otherwise.
      - The format of the SAN OID definition is `2.5.29.17={text}token=value&token=value...` where token can be one of the following:
        - **UPN.** A user principal name in the following format: admin@contoso.com
        - **Email.** An email address, such as this example: admin@contoso.com
        - **DNS.** A computer name in the following format: computer.contoso.com
        - **DirectoryName.** CN=Name,DC=Domain,DC=com
        - **URL.** The URL of a host, such as this example: http://computer07.contoso.com/index.html
        - **IPAddress.** An IP address
        - **RegisteredID.** ID in dotted decimal notation, such as this example: 1.2.3.4.5
        - **GUID.** A globally unique ID, such as this example: f7c3ac41-b8ce-4fb4-aa58-3d1dc0e36b39
      - More details about the command and this specific parameter can be found in the `New-SelfSignedCertificate` documentation referenced at the beginning of this section.

#### Trust the Self-Signed Certificate

Once the certificate is generated, you need to tell .NET Core to trust the certificate for use with the API. Since the certificate includes the special `1.3.6.1.4.1.311.84.1.1` OID, it will be recognized by the .NET Core tooling.

To trust the certificate, simply run:

`dotnet dev-certs https --trust`

and answer `Yes` to any prompts asking whether you want to trust the certificate. This will place a certificate with the same thumbprint and other properties into the `cert:/CurrentUser/CA` (or the CurrentUser > Trusted Root Certification Authorities) certificate store.

#### Export the Self-Signed Certificate

Now that the self-signed certificate is both generated and trusted, we need to export it for use with our Android application. In `certmgr.msc` right-click on the self-signed certificate and go to `All Tasks > Export`. Specify the file path within your Android project's source code for its raw resources. That is `Resources/raw`. You may have to create the `raw` folder if it does not yet exist.

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

### Define Debug Network Configuration for Android

In your Android project's `Resources/xml` folder, we need to specify a `network_security_config.xml` file. The file should have the following format with `<certificate name>` being the name of the exported certificate file:

```
<?xml version="1.0" encoding="utf-8" ?>
<network-security-config>
  <debug-overrides>
    <trust-anchors>
      <certificates src="@raw/<certificate name>"/>
    </trust-anchors>
  </debug-overrides>
</network-security-config>
```

This configuration creates an override for the network trust anchors when debugging your application, using the specified certificate. Please see the official Android documentation for more information about [debug CA's](https://developer.android.com/training/articles/security-config#TrustingDebugCa) and [HTTPS/SSL security for Android](https://developer.android.com/training/articles/security-ssl).

### Add Networking Configuration to Android Manifest

Next you will need to add the networking configuration file path to your Android manifest so that the trust anchor can be found. Do so by either editing the Android manifest directly, like so:

```
<?xml version="1.0" encoding="utf-8"?>
<manifest>
    <application android:networkSecurityConfig="@xml/network_security_config">
        ...
    </application>
</manifest>
```

or if you are using Xamarin.Forms, you can modify the main project's `AssemblyInfo.cs` file to make use of the [`ApplicationAttribute`](https://docs.microsoft.com/en-us/dotnet/api/android.app.applicationattribute?view=xamarin-android-sdk-9) and its [`NetworkSecurityConfig`](https://docs.microsoft.com/en-us/dotnet/api/android.app.applicationattribute.networksecurityconfig?view=xamarin-android-sdk-9#Android_App_ApplicationAttribute_NetworkSecurityConfig) property, like so:

```
//Add Network Config
[assembly: Application(NetworkSecurityConfig = "@xml/network_security_config")]
```

For Xamarin.Forms, the latter is the recommended practice. See [this document](https://docs.microsoft.com/en-us/xamarin/android/platform/android-manifest) for more information about dealing with the Android Manifest.

### Call the Web API from your Android code

Once all of these networking configurations are complete, you need to make sure to call the Web API with a proper host IP address of `10.0.2.2` from your Android code, instead of the usual `localhost`. Follow the instructions [here](https://docs.microsoft.com/en-us/xamarin/cross-platform/deploy-test/connect-to-local-web-services#specify-the-local-machine-address) to make sure you modify the hostname correctly.

## Bypass the certificate security check (not recommended for Android)

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

## Enable HTTP clear-text traffic (not recommended for Android)

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

## Related links

- [Android - Security with HTTPS and SSL](https://developer.android.com/training/articles/security-ssl)
- [Android - Network Security Configuration](https://developer.android.com/training/articles/security-config)
- [StackOverflow - .NET Core SSL Certificate Recognition and Validation](https://stackoverflow.com/a/65958649/13159009)
- [GitHub - ASP.NET Core - Source Code for `dotnet dev-certs` `CertificateManager` validation](https://github.com/dotnet/aspnetcore/blob/c4d043dc86e52e8b83f8c0ef3500f2985f90c23c/src/Shared/CertificateGeneration/CertificateManager.cs#L83)
- [PowerShell - New-SelfSignedCertificate Command (PKI)](https://docs.microsoft.com/en-us/powershell/module/pki/new-selfsignedcertificate?view=windowsserver2019-ps)
- [TodoREST (sample)](/samples/xamarin/xamarin-forms-samples/webservices-todorest/)
- [Enable local HTTPS](/aspnet/core/getting-started#enable-local-https)
- [HttpClient and SSL/TLS implementation selector for iOS/macOS](~/cross-platform/macios/http-stack.md)
- [HttpClient Stack and SSL/TLS Implementation selector for Android](~/android/app-fundamentals/http-stack.md)
- [Android Network Security Configuration](https://devblogs.microsoft.com/xamarin/cleartext-http-android-network-security/)
- [iOS App Transport Security](~/ios/app-fundamentals/ats.md)
- [Xamarin.Essentials: Device Information](~/essentials/device-information.md)
