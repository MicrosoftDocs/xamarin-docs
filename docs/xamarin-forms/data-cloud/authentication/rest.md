---
title: "Authenticate a RESTful Web Service"
description: "Basic authentication provides access to resources to only those clients that have the correct credentials. This article explains how to use basic authentication to protect access to RESTful web service resources."
ms.prod: xamarin
ms.assetid: 7B5FFDC4-F2AA-4B12-A30A-1DACC7FECBF1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/22/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Authenticate a RESTful Web Service

_HTTP supports the use of several authentication mechanisms to control access to resources. Basic authentication provides access to resources to only those clients that have the correct credentials. This article demonstrates how to use basic authentication to protect access to RESTful web service resources._

> [!NOTE]
> In iOS 9 and greater, App Transport Security (ATS) enforces secure connections between internet resources (such as the app's back-end server) and the app, thereby preventing accidental disclosure of sensitive information. Since ATS is enabled by default in apps built for iOS 9, all connections will be subject to ATS security requirements. If connections do not meet these requirements, they will fail with an exception.
> ATS can be opted out of if it is not possible to use the `HTTPS` protocol and secure communication for internet resources. This can be achieved by updating the app's **Info.plist** file. For more information see [App Transport Security](~/ios/app-fundamentals/ats.md).

## Authenticating Users over HTTP

Basic authentication is the simplest authentication mechanism supported by HTTP, and involves the client sending the username and password as unencrypted base64 encoded text. It works as follows:

- If a web service receives a request for a protected resource, it rejects the request with an HTTP status code 401 (access denied) and sets the WWW-Authenticate response header, as shown in the following diagram:

![Basic Authentication Failing](rest-images/basic-authentication-fail.png)

- If a web service receives a request for a protected resource, with the `Authorization` header correctly set, the web service responds with an HTTP status code 200, which indicates that the request succeeded and that the requested information is in the response. This scenario is shown in the following diagram:

![Basic Authentication Succeeding](rest-images/basic-authentication-success.png)

> [!NOTE]
> Basic authentication should only be used over an HTTPS connection. When used over an HTTP connection, the `Authorization` header can easily be decoded if the HTTP traffic is captured by an attacker.

## Specifying Basic Authentication in a Web Request

Use of basic authentication is specified as follows:

1. The string "Basic " is added to the `Authorization` header of the request.
1. The username and password are combined into a string with the format "username:password", which is then base64 encoded and added to the `Authorization` header of the request.

Therefore, with a username of 'XamarinUser' and a password of 'XamarinPassword', the header becomes:

```csharp
Authorization: Basic WGFtYXJpblVzZXI6WGFtYXJpblBhc3N3b3Jk
```

The `HttpClient` class can set the `Authorization` header value on the `HttpClient.DefaultRequestHeaders.Authorization` property. Because the `HttpClient` instance exists across multiple requests, the `Authorization` header needs only to be set once, rather than when making every request, as shown in the following code example:

```csharp
public class RestService : IRestService
{
  HttpClient _client;
  ...

  public RestService ()
  {
    var authData = string.Format ("{0}:{1}", Constants.Username, Constants.Password);
    var authHeaderValue = Convert.ToBase64String (Encoding.UTF8.GetBytes (authData));

    _client = new HttpClient ();
    _client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue ("Basic", authHeaderValue);
  }
  ...
}
```

Then when a request is made to a web service operation the request is signed with the `Authorization` header, indicating whether or not the user has permission to invoke the operation.

> [!IMPORTANT]
> While this code stores credentials as constants, they should not be stored in an insecure format in a published application.

## Processing the Authorization Header Server Side

The REST service should decorate each action with the `[BasicAuthentication]` attribute. This attribute is used to parse the `Authorization` header and determine if the base64 encoded credentials are valid by comparing them against values stored in *Web.config*. While this approach is suitable for a sample service, it requires extending for a public-facing web service.

In the basic authentication module used by IIS, users are authenticated against their Windows credentials. Therefore, users must have accounts on the server's domain. However, the Basic authentication model can be configured to allow custom authentication, where user accounts are authenticated against an external source, such as a database. For more information see [Basic Authentication in ASP.NET Web API](https://www.asp.net/web-api/overview/security/basic-authentication) on the ASP.NET website.

> [!NOTE]
> Basic authentication was not designed to manage logging out. Therefore, the standard basic authentication approach for logging out is to end the session.

## Related Links

- [Consume a RESTful web service](~/xamarin-forms/data-cloud/web-services/rest.md)
- [HttpClient](/dotnet/api/system.net.http.httpclient)