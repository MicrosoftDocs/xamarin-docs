---
title: "Microsoft Azure and Xamarin"
description: "This document links to documentation about Connected Services in Visual Studio for Mac, Azure Mobile Apps, Active Directory Authentication, and WebAPI."
ms.prod: xamarin
ms.assetid: 7b9aa8d9-c181-4c33-8ab0-2f56e4dbfc04
author: davidortinau
ms.author: daortin
ms.date: 10/09/2017
---

# Microsoft Azure and Xamarin

[![Azure App Services features are easy to add to Xamarin apps, including cloud data storage and cross-platform push notifications](images/evolve-mikej-azure-sml.png)](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

[Evolve 2016: Developing Connected Apps Using Azure and Xamarin](https://evolve.xamarin.com/session/56ec886fde91c6253c277bc6)

## Connected Services in Visual Studio for Mac

The new [Connected Services](/visualstudio/mac/connected-services) feature of Visual Studio for Mac
  helps developers to quickly and easily add Azure functionality to
  mobile applications from within the IDE. Currently available for testing in the Alpha channel.

## Azure App Services

There is a collection of [Azure Mobile Apps documentation](~/cross-platform/data-cloud/mobile-apps.md)
  that guides you through the process of implementing
  the [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).
  Xamarin also offers an Azure Messaging NuGet packages for [iOS](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.iOS/) and [Android](https://www.nuget.org/packages/Xamarin.Azure.NotificationHubs.Android/)
  to help implement push notifications across platforms.

Configure your apps on the [Azure App Services portal](https://portal.azure.com/)
  to access Mobile Apps, Web APIs, Storage, and much more. Learn about [how app services are different](https://azure.microsoft.com/updates/whats-new-with-azure-app-service/) and watch in
  [these videos from Microsoft](https://azure.microsoft.com/campaigns/azure-march-announcement/).

## Active Directory Authentication

[Azure Active Directory](~/cross-platform/data-cloud/active-directory/index.md)
  can be used to login users to Xamarin apps. The apps can then access additional services like Office 365.

## WebAPI

Microsoft's Web API exposes a REST-like interface
  that can be easily consumed by Xamarin applications.
  You can easily spin-up an [Azure Website](https://trywebsites.azurewebsites.net/)
  and build a WebAPI-based app to connect to Xamarin
  apps.

### [Introduction To Web Services](~/cross-platform/data-cloud/web-services/index.md)

This tutorial introduces how to integrate REST, WCF and SOAP web service
  technologies with Xamarin mobile applications. It examines various service
  implementations, evaluates available tools and libraries to integrate them,
  and provides sample patterns for consuming service data. Finally, it
  provides a basic overview of creating a RESTful web service for consumption
  with a Xamarin mobile application.

## Samples

In addition to the [documentation samples](https://github.com/xamarin/mobile-samples/tree/master/Azure),
the following complete applications demonstrate various Azure features
incorporated into Xamarin apps:

- [Sport](https://github.com/xamarin/Sport) – friendly sports-league tracking app that uses data storage & push notifications.
- [Moments](https://github.com/pierceboggan/Moments) – instant photo sharing that uses Azure Storage for images.
- [Xamarin CRM](https://github.com/xamarin/app-crm) – uses Web API for the back-end.
- [MyShoppe](https://github.com/jamesmontemagno/MyShoppe) - Azure Mobile Apps.

- [eShop](https://github.com/dotnet-architecture/eShopOnContainers) – sample for the [Architecture series](https://www.microsoft.com/net/learn/architecture) of ebooks.
- [MyDriving](https://azure.microsoft.com/campaigns/mydriving/) – Azure + IoT sample from Build 2016.

## Related Links

- [Azure PCL Example (by @paulbatum) (sample)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure portal](https://azure.microsoft.com/)
- [Mobile Client for Xamarin (NuGet)](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)