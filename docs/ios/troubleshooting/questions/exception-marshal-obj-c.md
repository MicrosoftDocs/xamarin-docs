---
title: "Why does my iOS 9 app fail with: System.Exception: Failed to marshal the Objective-C object?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8805ABEC-48D4-4CCB-A226-3A5B2ECE4BF0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
---

# Why does my iOS 9 app fail with: System.Exception: Failed to marshal the Objective-C object?

You may see an error of this form:

> System.Exception: Failed to marshal the Objective-C object ... Could not find an existing managed instance for this object ...

API changes in iOS 9 require that a callback constructor be used when calling unmanaged code, as the underlying API now expects it. Use the following line to add the callback constructor to the class: 

`public foo (IntPtr handle) : base (handle) ` 

### Next Steps

For further assistance, to contact us, or if this issue remains even after utilizing the above information, please see [What support options are available for Xamarin?](~/cross-platform/troubleshooting/support-options.md) for information on contact options, suggestions, as well as how to file a new bug if needed. 
