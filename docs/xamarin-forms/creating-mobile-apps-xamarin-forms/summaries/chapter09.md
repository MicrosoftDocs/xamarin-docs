---
title: "Summary of Chapter 9. Platform-specific API calls"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
---

# Summary of Chapter 9. Platform-specific API calls

It is sometimes necessary to run some code that varies by platform. This chapter explores the techniques.

## Preprocessing in the Shared Asset Project

A Xamarin.Forms Shared Asset Project can execute different code for each platform using the C# preprocessor directives `#if`, `#elif`, and `endif`. This is demonstrated in [**PlatInfoSap1**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap1):

[![Triple screenshot of variable formatted paragraph](images/ch09fg01-small.png "Device Model and Operating System")](images/ch09fg01-large.png#lightbox "Device Model and Operating System")

However, the resultant code can be ugly and difficult to read.

## Parallel classes in the Shared Asset Project

A more structured approach to executing platform-specific code in the SAP is demonstrated in the [**PlatInfoSap2**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/PlatInfoSap2) sample. Each of the platform projects contains an identically named class and methods, but implemented for that particular platform. The SAP then simply instantiates the class and calls the method.

## DependencyService and the Portable Class Library

A library cannot normally access classes in application projects. This restriction seems to prevent the technique shown in **PlatInfoSap2** from being used in a PCL. However, Xamarin.Forms contains a class named [`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) that uses .NET reflection to access public classes in the application project from the PCL.

The PCL must define an `interface` with the members it needs to use in each platform. Then, each of the platforms contains an implementation of that interface. The class that implements the interface must be identified with a [DependencyAttribute](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyAttribute/) on the assembly level.

The PCL then uses the generic [`Get`](https://developer.xamarin.com/api/member/Xamarin.Forms.DependencyService.Get{T}/p/Xamarin.Forms.DependencyFetchTarget/) method of `DependencyService` to obtain an instance of the platform class that implements the interface.

This is demonstrated in the [**DisplayPlatformInfo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/DisplayPlatformInfo) sample.

## Platform-specific sound generation

The [**MonkeyTapWithSound**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09/MonkeyTapWithSound) sample adds beeps to the **MonkeyTap** program by accessing sound-generation facilities in each platform.



## Related Links

- [Chapter 9 full text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch09-Apr2016.pdf)
- [Chapter 9 samples](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter09)
- [Dependency Service](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
