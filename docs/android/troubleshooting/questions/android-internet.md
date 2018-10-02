---
title: "Why can't my Android release build connect to the Internet?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
---

# Why can't my Android release build connect to the Internet?

## Cause

The most common cause of this issue is that the **INTERNET** permission
is automatically included in a debug build, but must be set manually
for a release build. This is because the Internet permission is used to
allow a debugger to attach to the process, as described for
"DebugSymbols" [here](~/android/deploy-test/building-apps/build-process.md).


## Fix

To resolve the issue, you can require the Internet permission in the
Android Manifest. This can be done either through the manifest editor
or the manifest's sourcecode:

-   Fix in Editor: In your Android project, go to **Properties ->
    AndroidManifest.xml -> Required Permissions** and check
    **Internet**

-   Fix in Sourcecode: Open the AndroidManifest in a source editor and
    add the permission tag inside the `<Manifest>` tags:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
