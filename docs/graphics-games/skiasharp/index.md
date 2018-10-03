---
redirect_url: /xamarin/xamarin-forms/user-interface/graphics/skiasharp/
title: "2D Drawing With SkiaSharp"
description: "This document provides an overview of cross-platform 2D drawing with SkiaSharp. It links to various guides that describe SkiaSharp and its various APIs."
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: davidbritch
ms.author: dabritch
ms.date: 07/17/2018
---

# 2D drawing With SkiaSharp

SkiaSharp provides a powerful C# API for doing 2D graphics. It is
powered by [Google’s Skia library](http://skia.org), the
same library that powers Google Chrome, Firefox and Android’s graphic
stacks.

[![](images/ide-sml.png "SkiaSharp provides a powerful C# API for doing 2D graphics")](images/ide.png#lightbox)

SkiaSharp is a Portable Library, and ships conveniently as a
[cross-platform NuGet package](https://www.nuget.org/packages/SkiaSharp),
and supports the following platforms out of the box:
macOS, Xamarin.Android, Xamarin.iOS, and the Windows Desktop.

## [Introduction to SkiaSharp](~/graphics-games/skiasharp/introduction.md)

An overview of the core concepts of SkiaSharp and sample code to render
graphics, text, bitmaps, and use image filters.

## [SkiaSharp Tutorials for Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Learn how to work with cross platform graphics that render in Xamarin.Forms.

## [Platform Specific Notes](~/graphics-games/skiasharp/platform.md)

This page describes the setup instructions for SkiaSharp on different platforms
including iOS, Android, macOS, and Windows.

## [API Documentation](https://docs.microsoft.com/dotnet/api/skiasharp)

You can browse the [API documentation](https://docs.microsoft.com/dotnet/api/skiasharp) for SkiaSharp.

## Work in Progress

SkiaSharp is a work in progress that we are sharing with our
community. While we have bound important parts of the Skia API, much
work remains to be done. We are using the stable C API surfaced by
Skia, and our plan is to continue contributing our work to the C
bindings of Skia to provide full coverage to the APIs.

To help us guide our binding efforts, please leave comments or
suggestions as issues on the GitHub repository
[http://github.com/mono/SkiaSharp](http://github.com/mono/SkiaSharp).
