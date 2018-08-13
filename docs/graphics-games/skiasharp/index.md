---
title: "2D Drawing With SkiaSharp"
description: "This document provides an overview of cross-platform 2D drawing with SkiaSharp. It links to various guides that describe SkiaSharp and its various APIs."
ms.prod: xamarin
ms.techonology: xamarin-skiasharp
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
author: charlespetzold
ms.author: chape
ms.date: 07/17/2018
---

# 2D Drawing With SkiaSharp

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

Learn how to work with cross platform graphics that render in Xamarin.Forms:

- [Drawing Basics](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Drawing a simple circle](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Integrating with Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Pixels and device-independent units](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Basic animation](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Integrating text and graphics](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Bitmap basics](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Lines and Paths](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Lines and stroke caps](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Path basics](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [The path fill types](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Polylines and parametric equations](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Dots and dashes](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Finger Painting](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Transforms](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [The translate transform](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [The scale transform](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [The rotate transform](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [The skew transform](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Matrix transforms](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Touch manipulations](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Non-affine transforms](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [3D rotation](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Curves and Paths](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Three Ways to Draw an Arc](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Three Types of Bézier Curves](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [SVG Path Data](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Clipping with Paths and Regions](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Path Effects](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Paths and Text](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Path Information and Enumeration](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)
- [Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/index.md)
  * [Displaying Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/displaying.md)
  * [Creating and Drawing on Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/drawing.md)
  * [Cropping Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/cropping.md)
  * [Segmented Display of Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/segmented.md)
  * [Saving Bitmaps to Files](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/saving.md)
  * [Accessing Bitmap Pixel Bits](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/pixel-bits.md)
  * [Animating Bitmaps](~/xamarin-forms/user-interface/graphics/skiasharp/bitmaps/animating.md)

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
