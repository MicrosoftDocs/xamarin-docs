---
title: "macOS APIs for Xamarin.Mac Developers"
description: "This document describes how to read Objective-C selectors and how to find their corresponding C# methods in a Xamarin.Mac app."
ms.prod: xamarin
ms.assetid: 9F7451FA-E07E-4C7B-B5CF-27AFC157ECDA
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/02/2017
---

# macOS APIs for Xamarin.Mac Developers

## Overview

For much of your time developing with Xamarin.Mac, you can think, read, and write in C# without much concern with the underlying Objective-C APIs. However, sometimes you’ll need to read the API documentation from Apple, translate an answer from Stack Overflow to a solution for your problem, or compare to an existing sample.

## Reading enough Objective-C to be dangerous

Sometimes it will be necessary to read an Objective-C definition or method call and translate that to the equivalent C# method. Let’s take a look at an Objective-C function definition and break down the pieces. This method (a *selector* in Objective-C) can be found on `NSTableView`:

```objc
- (BOOL)canDragRowsWithIndexes:(NSIndexSet *)rowIndexes atPoint:(NSPoint)mouseDownPoint
```

The declaration can be read left to right:

- The `-` prefix means it is an instance (non-static) method. + means it is a class (static) method
- `(BOOL)` is the return type (bool in C#)
- `canDragRowsWithIndexes` is the first part of the name.
- `(NSIndexSet *)rowIndexes` is the first param and with it’s type. The first parameter is in the format: `(Type) paramName`
- `atPoint:(NSPoint)mouseDownPoint` is the second param and its type. Every parameter after the first is the format: `selectorPart:(Type) paramName`
- The full name of this message selector is: `canDragRowsWithIndexes:atPoint:`. Note the `:` at the end - it is important.
- The actual Xamarin.Mac C# binding is: `bool CanDragRows (NSIndexSet rowIndexes, PointF mouseDownPoint)`

This selector invocation can be read the same way:

```objc
[v canDragRowsWithIndexes:set atPoint:point];
```

- The instance `v` is having its `canDragRowsWithIndexes:atPoint` selector called with two parameters, `set` and `point`, passed in.
- In C#, The method invocation looks like this: `v.CanDragRows (set, point);`

<a name="finding_selector"></a>

## Finding the C# member for a given selector

Now that you’ve found the Objective-C selector you need to invoke, the next step is mapping that to the equivalent C# member. There are four approaches you can try (continuing with the `NSTableView CanDragRows` example):

1. Use the auto completion list to quickly scan for something of the same name. Since we know it is an instance of `NSTableView` you can type:

    - `NSTableView x;`
    - `x.` [ctrl+space if the list does not appear).
    - `CanDrag` [enter]
    - Right-click the method, go to declaration to open the Assembly Browser where you can compare the `Export` attribute to the selector in question

2. Search the entire class binding. Since we know it is an instance of `NSTableView` you can type:

    - `NSTableView x;`
    - Right-click `NSTableView`, go to declaration to Assembly Browser
    - Search for the selector in question

3. You can use the [Xamarin.Mac API online documentation](/dotnet/api/?view=xamarinmac-3.0) .

4. Miguel provides a "Rosetta Stone" view of the Xamarin.Mac APIs [here](https://tirania.org/tmp/rosetta.html) that you can search through for a given API. If your API is not AppKit or macOS specific, you may find it there.

<!--
Note: In some cases, the assembly browser can hit a bug where it will open but not jump to the right definition. Keep that tab open, switch back to your source code and try again.
Note: The assembly browser tricks currently only works with Xamarin.Mac Classic. This will be fixed in a future version.
-->
