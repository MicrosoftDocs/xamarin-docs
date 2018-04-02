---
title: "SkiaSharp Lines and Paths"
description: "Use SkiaSharp to draw lines and graphics paths"
ms.topic: article
ms.prod: xamarin
ms.assetid: 316A15FE-383D-4D06-8641-BAC7EE7474CA
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
---

# SkiaSharp Lines and Paths

_Use SkiaSharp to draw lines and graphics paths_

The [previous section](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md) demonstrated that the SkiaSharp `SKCanvas` class includes several methods to draw rectangles, ellipses, and rounded rectangles. This section and later sections cover the various classes connected with creating and rendering *graphics paths*.

The graphics path is the most generalized approach to drawing lines and curves in SkiaSharp. This section covers using an `SKPath` object to draw straight lines, and to use a collection of tiny straight lines (called a *polyline*) to draw curves that you can define mathematically. A later section will discusses the various sorts of curves supported by `SKPath`.

All the sample programs in this section appear under the heading **Lines and Paths** in the home page of the [**SkiaSharpFormsDemos**](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) program, and in the [**Paths**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/SkiaSharpFormsDemos/SkiaSharpFormsDemos/SkiaSharpFormsDemos/Paths) folder of the solution.

## [Lines and Stroke Caps](lines.md)

Learn how to use SkiaSharp to draw lines with different stroke caps.

## [Path Basics](paths.md)

Explore the SkiaSharp SKPath object for combining lines and curves.

## [The Path Fill Types](fill-types.md)

Discover the different effects possible with SkiaSharp path fill types.

## [Polylines and Parametric Equations](polylines.md)

Use SkiaSharp to render any line you can define with parametric equations.

## [Dots and Dashes](dots.md)

Master the intricacies of drawing dotted and dashed lines in SkiaSharp.

## [Finger Painting](finger-paint.md)

Use your fingers to paint on the canvas.


## Related Links

- [SkiaSharp APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (sample)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
