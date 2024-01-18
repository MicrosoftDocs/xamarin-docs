---
title: "Where are the components stored on my machine?"
description: Learn how to locate where a list of components are stored.
ms.topic: troubleshooting
ms.service: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
no-loc: [Objective-C]
---

# Where are the components stored on my machine?

Whenever you install a Xamarin component into an App project, it gets placed in two places:

1. In a Components folder at the root level of your Solution folder. If you remove the Component from all projects in the solution, it will get removed from this folder as well.

2. A copy is also stored in the following locations:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

So to remove a component completely from your system, delete it from your projects/solutions and from the cache folder above.
