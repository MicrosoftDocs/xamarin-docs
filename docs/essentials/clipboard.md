---
title: "Xamarin.Essentials Clipboard"
description: "The Clipboard class lets you copy and paste text to the system clipboard between applications."
ms.assetid: C52AE99A-0FB3-425D-9106-3DA5777FEFA0
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---
# Xamarin.Essentials Clipboard

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Clipboard** class lets you copy and paste text to the system clipboard between applications.

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
Clipboard.SetText("Hello World");
```

To read text from the **Clipboard**:

```csharp
var text = await Clipboard.GetTextAsync();
```

## API

- [Clipboard source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Clipboard)
- [Clipboard API documentation](xref:Xamarin.Essentials.Clipboard)
