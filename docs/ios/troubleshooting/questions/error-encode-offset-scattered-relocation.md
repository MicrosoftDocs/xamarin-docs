---
title: "Compile error: Can not encode offset X in resulting scattered relocation"
description: Learn about the compile error message that is received when the final binary is too large for the native toolchain.
ms.topic: troubleshooting
ms.service: xamarin
ms.assetid: 84158C42-FE79-415A-A44A-D48C9E5E6760
ms.subservice: xamarin-ios
author: chamons
ms.author: chhamo
ms.date: 08/07/2019
no-loc: [Objective-C]
---

# Compile error: Can not encode offset X in resulting scattered relocation

```
1>C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Xamarin\iOS\Xamarin.iOS.Common.targets(804,3): error GDC116A36: can not encode offset ‘0x1155504’ in resulting scattered relocation.
1> .long mono_aot_Xamarin_iOS_got - . + 12 (TaskId:141)
1> ^ (TaskId:141)
1>C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Xamarin\iOS\Xamarin.iOS.Common.targets(804,3): error GDC116A36: can not encode offset ‘0x11555B8’ in resulting scattered relocation.
1> .long mono_aot_Xamarin_iOS_got - . + 16 (TaskId:141)
```

This issue occurs when building for 32-bit architectures, such as ARMv7, when the final binary is too large for the native toolchain.

To resolve this:

- Drop building for that architecture in the iOS Build pane of project options.
- Enable linking or manually remove code to reduce final executable size
