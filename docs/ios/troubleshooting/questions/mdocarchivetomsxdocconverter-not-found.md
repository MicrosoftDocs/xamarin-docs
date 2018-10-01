---
title: "MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5AC6AC4-0E7C-4746-A7CF-872F0E75AFF4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
---

# MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest

> [!IMPORTANT]
> This issue has been resolved in recent versions of Xamarin. However, if the issue occurs on the latest version of the software, please file a [new bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md) with your full versioning information and full build log output.


## Error message

This error may appear in the *Mac Server Log* in Visual Studio:

```
Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found
 rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context, System.Object commandRequestState) [0x00000] in <filename unknown>:0
  at Mtb.Server.Listener.OnRequest (System.Object state) [0x00000] in <filename unknown>:0
```

There are 2 separate issues in this message:

1.  `Error: /Developer/MonoTouch/usr/share/doc/MonoTouch/MDocArchiveToMsxDocConverter.exe not found`

    This error is harmless, but it is also misleading. It [will be removed](https://bugzilla.xamarin.com/show_bug.cgi?id=21667) in a future release.

2.  `rver.BaseCommand.OnRequest (System.Net.HttpListenerContext context …`

    This error is the real problem. Unfortunately, due to a [limitation](https://bugzilla.xamarin.com/show_bug.cgi?id=22080) this exception stack trace is *incomplete*. If you notice an incomplete stack trace like this in the Mac Server Log, you can check the `~/Library/Logs/Xamarin/MonoTouchVS/mtbserver.log` file on the Mac build host to find the complete stack trace.
