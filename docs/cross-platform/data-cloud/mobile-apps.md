---
title: "Microsoft Azure Mobile Apps"
description: "Samples and code downloads for the Azure portal documentation."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
---

# Microsoft Azure Mobile Apps

_Samples and code downloads for the Azure portal documentation._

<!--
NOTE TO AUTHORS: this page is referenced from
http://azure.microsoft.com/en-us/develop/mobile/xamarin/
as https://developer.xamarin.com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started	 http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data	http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push	http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication	http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs	http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data 	http://go.microsoft.com/fwlink/p/?LinkId=331330
-->


These links are for the Xamarin documentation available on the [Azure Mobile Apps](https://azure.microsoft.com/en-us/documentation/services/app-service/mobile/) website.
Adding Azure functionality to a Xamarin app is as simple as downloading the [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## Working with the Xamarin Azure Component

General documentation [Working with the Xamarin Client Library (Component)](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-dotnet-how-to-use-client-library/) to accomplish various tasks with Azure Mobile Apps. This page contains lots of sample code snippets, without the detailed explanations and examples available in each of the walkthrough articles listed below.

## Getting Started

This article provides step-by-step instructions to get your first Xamarin Azure app up and running.
It covers creating a new Azure Mobile App in the portal and then downloading and running the pre-configured app.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started/)
-  [Xamarin.Forms](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-forms-get-started/)

## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)


## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)


## Get Started with Users

Provides complete instructions for configuring and coding a Login screen using Azure Mobile Services. Supported authentication providers include Microsoft, Google, Facebook, and Twitter.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)


## Authorize Users in Scripts

Some sample code for Javascript backends

-  [Todo.js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)


## Get Started with Push

Complete instructions to configure push notifications on the Apple and Google websites, then send a push notification from Azure Mobile Services to a device.

-  [iOS](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-ios-get-started-push/)
-  [Android](https://azure.microsoft.com/en-us/documentation/articles/app-service-mobile-xamarin-android-get-started-push/)


## Get Started with Notification Hubs

Complete instructions to configure push notifications on the Apple and Google websites, configure the Azure Notification Hub and then generate push notifications to devices.

-  [iOS](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-ios-get-started/)
-  [Android](http://azure.microsoft.com/en-us/documentation/articles/partner-xamarin-notification-hubs-android-get-started/)



## Related Links

- [GettingStarted (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [GetStartedWithUsers (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [GetStartedWithPush (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
- [NotificationHubs (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Azure PCL Example (by @paulbatum) (sample)](https://github.com/paulbatum/mobile-services-xamarin-pcl)
- [Azure Mobile Client](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Azure Mobile Apps learning path](https://azure.microsoft.com/en-us/documentation/learning-paths/appservice-mobileapps/)
