---
title: "Authenticating Access to Web Services"
description: "This guide explains how to integrate authentication services into a Xamarin.Forms application to enable users to share a backend while only having access to their own data. Topics covered include using basic authentication with a REST service, using the Xamarin.Auth component to authenticate against OAuth identity providers, and using the built-in authentication mechanisms offered by different providers."
ms.prod: xamarin
ms.assetid: E6FCFAE1-4F83-4F93-9190-EC5290360C54
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
---

# Authenticating Access to Web Services

_This guide explains how to integrate authentication services into a Xamarin.Forms application to enable users to share a backend while only having access to their own data. Topics covered include using basic authentication with a REST service, using the Xamarin.Auth component to authenticate against OAuth identity providers, and using the built-in authentication mechanisms offered by different providers._

## [Authenticating a RESTful Web Service](rest.md)

HTTP supports the use of several authentication mechanisms to control access to resources. Basic authentication provides access to resources to only those clients that have the correct credentials. This article demonstrates how to use basic authentication to protect access to RESTful web service resources.

## [Authenticating Users with an Identity Provider](oauth.md)

Xamarin.Auth is a cross-platform SDK for authenticating users and storing their accounts. It includes OAuth authenticators that provide support for consuming identity providers such as Google, Microsoft, Facebook, and Twitter. This article explains how to use Xamarin.Auth to manage the authentication process in a Xamarin.Forms application.

## [Authenticating Users with Azure Mobile Apps](azure.md)

Azure Mobile Apps use a variety of external identity providers to support authenticating and authorizing application users. Permissions can then be set on tables to restrict access to only authenticated users. This article explains how to use Azure Mobile Apps to manage the authentication process in a Xamarin.Forms application.

## [Authenticating Users with Azure Active Directory B2C](azure-ad-b2c.md)

Azure Active Directory B2C is a cloud identity management solution for consumer-facing web and mobile applications. This article demonstrates how to use Microsoft Authentication Library (MSAL) and Azure Active Directory B2C to integrate consumer identity management into a Xamarin.Forms application.

## [Integrating Azure Active Directory B2C with Azure Mobile Apps](azure-ad-b2c-mobile-app.md)

Azure Active Directory B2C can be used to manage the authentication workflow for Azure Mobile Apps. With this approach, the identity management experience is fully defined in the cloud, and can be modified without changing your mobile application code. This article demonstrates how to use Azure Active Directory B2C to provide authentication and authorization to an Azure Mobile Apps instance with Xamarin.Forms.

## Related Links

- [Introduction to Web Services](~/cross-platform/data-cloud/web-services/index.md)
- [Async Support Overview](~/cross-platform/platform/async.md)
