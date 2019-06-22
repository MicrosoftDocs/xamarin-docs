---
title: "Xamarin.Essentials: Clipboard"
description: "This document describes the Clipboard class in Xamarin.Essentials, which lets you copy and paste text to the system clipboard between applications."
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 02/12/2019
ms.custom: video
---

# Xamarin.Essentials: Clipboard

The **Clipboard** class lets you copy and paste text to the system clipboard between applications.

## Get started

[!include[](~/essentials/includes/get-started.md)]

## Using Clipboard

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

To check if the **Clipboard** has text currently ready to be pasted:

```csharp
var hasText = Clipboard.HasText;
```

To set text to the **Clipboard**:

```csharp
await Clipboard.SetTextAsync("Hello World");
```

To read text from the **Clipboard**:

```csharp
var text = await Clipboard.GetTextAsync();
```

> [!TIP]
> Access to the Clipboard must be done on the main user interface thread. See the [MainThread](~/essentials/main-thread) API to see how to invoke methods on the main user interface thread.

## API

- [Clipboard source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Clipboard API documentation](xref:Xamarin.Essentials.Clipboard)

## Related Video

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Clipboard-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
