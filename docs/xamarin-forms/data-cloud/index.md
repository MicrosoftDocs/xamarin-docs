---
title: "Data & Cloud Services"
description: "Xamarin.Forms applications can consume web services implemented using a wide variety of technologies, and this guide will examine how to do this."
ms.prod: xamarin
ms.assetid: 0601D9D0-C8D2-4C3B-A749-A340BDBF64A4ß
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
---

# Data & Cloud Services

_Xamarin.Forms applications can consume web services implemented using a wide variety of technologies, and this guide will examine how to do this._

For an introduction to cross-platform web service consumption on the Xamarin platform, see [Introduction to Web Services](~/cross-platform/data-cloud/web-services/index.md).

## [Understanding the Sample](~/xamarin-forms/data-cloud/walkthrough.md)

This article provides a walkthrough of the Xamarin.Forms sample application that demonstrates how to communicate with different web services. Topics covered include the anatomy of the application, the pages, data model, and invoking web service operations.

## [Consuming Web Services](~/xamarin-forms/data-cloud/consuming/index.md)

This guide demonstrates how to communicate with different web services to provide create, read, update, and delete (CRUD) functionality to a Xamarin.Forms application. Topics covered include communicating with [ASMX services](consuming/asmx.md), [WCF services](consuming/wcf.md), [REST services](consuming/rest.md), and [Azure Mobile Apps](consuming/azure.md).

## [Authenticating Access to Web Services](~/xamarin-forms/data-cloud/authentication/index.md)

This guide explains how to integrate authentication services into a Xamarin.Forms application to enable users to share a backend while only having access to their own data. Topics covered include [using basic authentication with a REST service](authentication/rest.md), [using the Xamarin.Auth component to authenticate against OAuth identity providers](authentication/oauth.md), and using the inbuilt authentication mechanisms offered by [Azure Mobile Apps](authentication/azure.md).

## [Synchronizing Data with Web Services](sync/index.md)

This article explains how to add offline sync functionality to a Xamarin.Forms application. Offline sync allows users to interact with a mobile application, viewing, adding, or modifying data, even where there isn't a network connection. Changes are stored in a local database, and once the device is online, the changes can be synced with the web service.

## [Sending Push Notifications](push-notifications/index.md)

This article demonstrates how to add push notifications to a Xamarin.Forms application. Azure Notification Hubs provide a scalable push infrastructure for sending mobile push notifications from any backend to any mobile platform, while eliminating the complexity of a backend having to communicate with different platform notification systems.

## [Storing Files in the Cloud](storage/index.md)

This article demonstrates how to use Xamarin.Forms to store text and binary data in Azure Storage, and how to access the data. Azure Storage is a scalable cloud storage solution that can be used for storing unstructured, and structured data.

## [Searching Data in the Cloud](search/index.md)

This article demonstrates how to use the Microsoft Azure Search Library to integrate Azure Search into a Xamarin.Forms application. Azure Search is a cloud service that provides indexing and querying capabilities for uploaded data. This removes the infrastructure requirements and search algorithm complexities traditionally associated with implementing search functionality in an application.

## [Storing Data in a Document Database](cosmosdb/index.md)

This guide demonstrates how to use the Azure Cosmos DB .NET Standard client library to integrate an Azure Cosmos DB document database into a Xamarin.Forms application. An Azure Cosmos DB document database is a NoSQL database that provides low latency access to JSON documents, offering a fast, highly available, scalable database service for applications that require seamless scale and global replication.

## [Adding Intelligence with Cognitive Services](cognitive-services/index.md)

This guide explains how to use some of the Microsoft Cognitive Services APIs in a Xamarin.Forms application. Cognitive Services are a set of APIs, SDKs, and services available to developers to make their applications more intelligent by adding features such as facial recognition, speech recognition, and language understanding.
