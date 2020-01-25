---
title: "Xamarin.Essentials Platform Extensions"
description: "Xamarin.Essentials provides several platform extension methods when having to work with platform types such as Rect, Size, and Point."
ms.assetid: AB4D198A-4FD7-479E-8627-01F887A6D056
author: jamesmontemagno
ms.author: jamont
ms.date: 03/13/2019
---

# Xamarin.Essentials: Platform Extensions

Xamarin.Essentials provides several platform extension methods when having to work with platform types such as Rect, Size, and Point. This means that you can convert between the `System` version of these types for their iOS, Android, and UWP specific types. 

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using Platform Extensions

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

All platform extensions can only be called from the iOS, Android, or UWP project.

### Point

```csharp
var system = new System.Drawing.Point(x, y);

// Convert to CoreGraphics.CGPoint, Android.Graphics.Point, and Windows.Foundation.Point
var platform = system.ToPlatformPoint();

// Back to System.Drawing.Point
var system2 = platform.ToSystemPoint();
```

### Size

```csharp
var system = new System.Drawing.Size(width, height);

// Convert to CoreGraphics.CGSize, Android.Util.Size, and Windows.Foundation.Size
var platform = system.ToPlatformSize();

// Back to System.Drawing.Size
var system2 = platform.ToSystemSize();
```

### Rectangle

```csharp
var system = new System.Drawing.Rectangle(x, y, width, height);

// Convert to CoreGraphics.CGRect, Android.Graphics.Rect, and Windows.Foundation.Rect
var platform = system.ToPlatformRectangle();

// Back to System.Drawing.Rectangle
var system2 = platform.ToSystemRectangle();
```

## API

- [Converters source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Types/PlatformExtensions)
- [Point Converters API documentation](xref:Xamarin.Essentials.PointExtensions)
- [Rectangle Converters API documentation](xref:Xamarin.Essentials.RectangleExtensions)
- [Size Converters API documentation](xref:Xamarin.Essentials.SizeExtensions)
