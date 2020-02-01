---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: "Common Tasks Comparison"
description: "This document compares how to perform various common tasks on WPF and Xamarin.Forms. It looks at buttons, timers, font sizes, opening a URI, and displaying an action sheet."
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
---

# Common Tasks Comparison

| Task | WPF | Xamarin.Forms |
|--- |--- |--- |
|Display a message on the screen with buttons|`MessageBox`|`Page.DisplayAlert`|
|Create a timer|`DispatcherTimer` class|`Device.StartTimer` static method|
|Get a default font size|`SystemFonts` static class|`Device.GetNamedSize` static method|
|Open a URI/URL|`Process.Start`|`Device.OpenUri`|
|Display action sheet (list of buttons)|n/a|`Page.DisplayActionSheet`|
