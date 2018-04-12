---
title: "Consuming Web Services"
description: "This guide demonstrates how to communicate with different web services to provide create, read, update, and delete (CRUD) functionality to a Xamarin.Forms application. Topics covered include communicating with ASMX services, WCF services, REST services, and Azure Mobile Apps."
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
---

# Consuming Web Services

_This guide demonstrates how to communicate with different web services to provide create, read, update, and delete (CRUD) functionality to a Xamarin.Forms application. Topics covered include communicating with ASMX services, WCF services, REST services, and Azure Mobile Apps.

## [Consuming an ASP.NET Web Service (ASMX)](~/xamarin-forms/data-cloud/consuming/asmx.md)

ASP.NET Web Services (ASMX) provide the ability to build web services that send messages over HTTP using Simple Object Access Protocol (SOAP). SOAP is a platform-independent and language-independent protocol for building and accessing web services. Consumers of an ASMX service do not need to know anything about the platform, object model, or programming language used to implement the service. They only need to understand how to send and receive SOAP messages. This article demonstrates how to consume an ASMX web service from a Xamarin.Forms application.

## [Consuming a Windows Communication Foundation (WCF) Web Service](~/xamarin-forms/data-cloud/consuming/wcf.md)

WCF is Microsoft's unified framework for building service-oriented applications. It enables developers to build secure, reliable, transacted, and interoperable distributed applications. There are differences between ASP.NET Web Services (ASMX) and WCF, but it is important to understand that WCF supports the same capabilities that ASMX provides — SOAP messages over HTTP. This article demonstrates how to consume an WCF SOAP service from a Xamarin.Forms application.

## [Consuming a RESTful Web Service](~/xamarin-forms/data-cloud/consuming/rest.md)

Representational State Transfer (REST) is an architectural style for building web services. REST requests are made over HTTP using the same HTTP verbs that web browsers use to retrieve web pages and to send data to servers. This article demonstrates how to consume a RESTful web service from a Xamarin.Forms application.

## [Consuming an Azure Mobile App](~/xamarin-forms/data-cloud/consuming/azure.md)

Azure Mobile Apps allow you to develop apps with scalable backends hosted in Azure App Service, with support for mobile authentication, offline sync, and push notifications. This article, which is only applicable to Azure Mobile Apps that use a Node.js backend, explains how to query, insert, update, and delete data stored in a table in an Azure Mobile Apps instance.

## Related Links

- [Introduction to Web Services](~/cross-platform/data-cloud/web-services/index.md)
- [Async Support Overview](~/cross-platform/platform/async.md)
