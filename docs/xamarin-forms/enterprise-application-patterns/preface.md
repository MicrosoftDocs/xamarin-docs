---
title: "Preface to Enterprise App Development"
description: "This chapter provides a preface to Enterprise Application Patterns using Xamarin.Forms."
ms.prod: xamarin
ms.assetid: fbf32a44-1d33-4e16-a904-dc7ee5991e7c
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Preface to Enterprise App Development

> [!NOTE]
> This eBook was published in the spring of 2017, and has not been updated since then. There is much in the book that remains valuable, but some of the material is outdated.

This eBook provides guidance on building cross-platform enterprise apps using Xamarin.Forms. Xamarin.Forms is a cross-platform UI toolkit that allows developers to easily create native user interface layouts that can be shared across platforms, including iOS, Android, and the Universal Windows Platform (UWP). It provides a comprehensive solution for Business to Employee (B2E), Business to Business (B2B), and Business to Consumer (B2C) apps, providing the ability to share code across all target platforms and helping to lower the total cost of ownership (TCO).

The guide provides architectural guidance for developing adaptable, maintainable, and testable Xamarin.Forms enterprise apps. Guidance is provided on how to implement MVVM, dependency injection, navigation, validation, and configuration management, while maintaining loose coupling. In addition, there's also guidance on performing authentication and authorization with IdentityServer, accessing data from containerized microservices, and unit testing.

The guide comes with source code for the [eShopOnContainers mobile app](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Mobile), and source code for the [eShopOnContainers reference app](https://github.com/dotnet-architecture/eShopOnContainers). The eShopOnContainers mobile app is a cross-platform enterprise app developed using Xamarin.Forms, which connects to a series of containerized microservices known as the eShopOnContainers reference app. However, the eShopOnContainers mobile app can be configured to consume data from mock services for those who wish to avoid deploying the containerized microservices.

## What's Left Out of this Guide's Scope

This guide is aimed at readers who are already familiar with Xamarin.Forms. For a detailed introduction to Xamarin.Forms, see the [Xamarin.Forms documentation](~/xamarin-forms/index.yml), and [Creating Mobile Apps with Xamarin.Forms](https://aka.ms/xamformsebook).

The guide is complementary to [.NET Microservices: Architecture for Containerized .NET Applications](https://aka.ms/microservicesebook), which focuses on developing and deploying containerized microservices. Other guides worth reading include [Architecting and Developing Modern Web Applications with ASP.NET Core and Microsoft Azure](https://aka.ms/WebAppEbook), [Containerized Docker Application Lifecycle with Microsoft Platform and Tools](https://aka.ms/dockerlifecycleebook), and [Microsoft Platform and Tools for Mobile App Development](https://aka.ms/MobAppDev/StndPDF).

## Who Should Use this Guide

The audience for this guide is mainly developers and architects who would like to learn how to architect and implement cross-platform enterprise apps using Xamarin.Forms.

A secondary audience is technical decision makers who would like to receive an architectural and technology overview before deciding on what approach to select for cross-platform enterprise app development using Xamarin.Forms.

## How to Use this Guide

This guide focuses on building cross-platform enterprise apps using Xamarin.Forms. As such, it should be read in its entirety to provide a foundation of understanding such apps and their technical considerations. The guide, along with its sample app, can also serve as a starting point or reference for creating a new enterprise app. Use the associated sample app as a template for the new app, or to see how to organize an app's component parts. Then, refer back to this guide for architectural guidance.

Feel free to forward this guide to team members to help ensure a common understanding of cross-platform enterprise app development using Xamarin.Forms. Having everybody working from a common set of terminologies and underlying principles will help ensure a consistent application of architectural patterns and practices.

## Related Links

- [Download eBook (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
