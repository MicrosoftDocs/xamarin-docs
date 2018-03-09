---
title: "Using SkiaSharp in Xamarin.Forms"
description: "Use SkiaSharp for 2D graphics in your Xamarin.Forms applications"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2C348BEA-81DF-4794-8857-EB1DFF5E11DB
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 09/11/2017
---

# Using SkiaSharp in Xamarin.Forms

_Use SkiaSharp for 2D graphics in your Xamarin.Forms applications_

SkiaSharp is a 2D graphics system for .NET and C# powered by the open-source Skia graphics engine that is used extensively in Google products. You can use SkiaSharp in your Xamarin.Forms applications to draw 2D vector graphics, bitmaps, and text. See the [2D Drawing](~/graphics-games/skiasharp/index.md) guide for more general information about the SkiaSharp library and some other tutorials.

This guide assumes that you are familiar with Xamarin.Forms programming.

> [!VIDEO https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms/player]

**Webinar: SkiaSharp for Xamarin.Forms**

## SkiaSharp Preliminaries

SkiaSharp for Xamarin.Forms is packaged as a NuGet package. After you've created a Xamarin.Forms solution in Visual Studio or Visual Studio for Mac, you can use the NuGet package manager to search for the **SkiaSharp.Views.Forms** package and add it to your solution. If you check the **References** section of each project after adding SkiaSharp, you can see that various **SkiaSharp** libraries have been added to each of the projects in the solution.

If your Xamarin.Forms application targets iOS, use the project properties page to change the minimum deployment target to iOS 8.0.

In any C# page that uses SkiaSharp you'll want to include a `using` directive for the [`SkiaSharp`](https://developer.xamarin.com/api/namespace/SkiaSharp/) namespace, which encompasses all the SkiaSharp classes, structures, and enumerations that you'll use in your graphics programming. You'll also want a `using` directive for the [`SkiaSharp.Views.Forms`](https://developer.xamarin.com/api/namespace/SkiaSharp.Views.Forms/) namespace for the classes specific to Xamarin.Forms. This is a much smaller namespace, with the most important class being [`SKCanvasView`](https://developer.xamarin.com/api/type/SkiaSharp.Views.Forms.SKCanvasView/). This class derives from the Xamarin.Forms `View` class and hosts your SkiaSharp graphics output.

> [!IMPORTANT]
> The `SkiaSharp.Views.Forms` namespace also contains an `SKGLView` class that derives from `View` but uses OpenGL for rendering graphics. For purposes of simplicity, this guide restricts itself to `SKCanvasView`, but using `SKGLView` instead is quite similar.

## [SkiaSharp Drawing Basics](basics/index.md)

Some of the simplest graphics figures you can draw with SkiaSharp are circles, ovals, and rectangles. In displaying these figures, you will learn about SkiaSharp coordinates, sizes, and colors.

## [SkiaSharp Lines and Paths](paths/index.md)

A graphics path is a series of connected straight lines and curves. Paths can be stroked, filled, or both. This topic encompasses many aspects of line drawing, including stroke ends and joins, and dashed and dotted lines, but stops short of curve geometries.

## [SkiaSharp Transforms](transforms/index.md)

Transforms allow graphics objects to be uniformly translated, scaled, rotated, or skewed. This article also shows how you can use a standard 3-by-3 transform matrix for creating non-affine transforms and applying transforms to paths.

## [SkiaSharp Curves and Paths](curves/index.md)

The exploration of paths continues with adding curves to a path objects, and exploiting other powerful path features. You'll see how you can specify an entire path in a concise text string, how to use path effects, and how to dig into path internals.


## Related Links

- [SkiaSharp APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SkiaSharpFormsDemos/)
- [SkiaSharp with Xamarin.Forms Webinar (video)](https://channel9.msdn.com/Events/Xamarin/Xamarin-University-Presents-Webinar-Series/SkiaSharp-Graphics-for-XamarinForms)
