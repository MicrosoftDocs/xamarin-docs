---
title: "Runtime error: The assembly mscorlib.dll was not found or could not be loaded"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 1027E16C-2C14-4BB5-AAAB-342F3E28E22E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
---

# Runtime error: The assembly mscorlib.dll was not found or could not be loaded

```
<Warning>: The assembly mscorlib.dll was not found or could not be loaded.
<Warning>: It should have been installed in the `/Developer/MonoTouch/Source/monotouch/builds/install/target64/lib/mono/2.0/mscorlib.dll' directory.
<Warning>: Service exited with abnormal code: 1
```

This issue occurs when the *hidden* `.monotouch-32` and `.monotouch-64` folders are missing from the `.xcarchive` for signing / IPA creation, triggering the runtime error.

