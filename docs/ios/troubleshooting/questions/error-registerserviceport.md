---
title: "iOS Designer Error with RegisterServicePort"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
---

# iOS Designer Error with RegisterServicePort

## Sample Error
> System.AggregateException: One or more errors occurred ---> System.SystemException: RegisterServicePort(com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): Kernel returned: -308 (-308): (ipc/mig) server died

## Explanation
Errors with `RegisterServicePort` and similar error messages like above are commonly an issue with spyware/malware on the computer. Please consider the [comment on this bug report](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) for more information, along with the link to the [Apple forum discussion](https://discussions.apple.com/thread/5596008) on how to remove a possible infection. 

To assist in diagnosing the issue, open up the macOS application **Console** and delete every file inside the **User diagnostic reports** section [https://screencast.com/t/y9i3NKcuMy](https://screencast.com/t/y9i3NKcuMy). Then start Visual Studio for Mac and try to use the designer. If any new log files appear in this section after the designer has failed to initialize, please save these for us to analyze.  

Please note the most important thing to check for is this file: 
> /usr/lib/libimckit.dylib

No matter the above results, if that file exists, the aforementioned spyware/malware issue is present on your computer.  

The following link has the steps to remove this spyware/malware: [https://www.thesafemac.com/arg-genieo/](https://www.thesafemac.com/arg-genieo/)  
