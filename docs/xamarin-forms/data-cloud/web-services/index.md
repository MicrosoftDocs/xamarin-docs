---
title: "Xamarin.Forms and Web Services"
description: "This guide explains how to communicate with different web services to provide create, read, update, and delete (CRUD) functionality to a Xamarin.Forms application. Topics covered include communicating with ASMX services, WCF services, REST services."
ms.prod: xamarin
ms.assetid: 8B360BDA-E4E3-4A3F-9004-0E35362F49F8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms and Web Services

## [Introduction](introduction.md)

This article provides a walkthrough of the Xamarin.Forms sample application that demonstrates how to communicate with different web services. Topics covered include the anatomy of the application, the pages, data model, and invoking web service operations.

## [Consume an ASP.NET Web Service (ASMX)](~/xamarin-forms/data-cloud/web-services/asmx.md)

ASP.NET Web Services (ASMX) provide the ability to build web services that send messages over HTTP using Simple Object Access Protocol (SOAP). SOAP is a platform-independent and language-independent protocol for building and accessing web services. Consumers of an ASMX service do not need to know anything about the platform, object model, or programming language used to implement the service. They only need to understand how to send and receive SOAP messages. This article demonstrates how to consume an ASMX web service from a Xamarin.Forms application.

## [Consume a Windows Communication Foundation (WCF) Web Service](~/xamarin-forms/data-cloud/web-services/wcf.md)

WCF is Microsoft's unified framework for building service-oriented applications. It enables developers to build secure, reliable, transacted, and interoperable distributed applications. There are differences between ASP.NET Web Services (ASMX) and WCF, but it is important to understand that WCF supports the same capabilities that ASMX provides — SOAP messages over HTTP. This article demonstrates how to consume an WCF SOAP service from a Xamarin.Forms application.

## [Consume a RESTful Web Service](~/xamarin-forms/data-cloud/web-services/rest.md)

Representational State Transfer (REST) is an architectural style for building web services. REST requests are made over HTTP using the same HTTP verbs that web browsers use to retrieve web pages and to send data to servers. This article demonstrates how to consume a RESTful web service from a Xamarin.Forms application.
