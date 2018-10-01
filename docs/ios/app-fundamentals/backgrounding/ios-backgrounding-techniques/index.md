---
title: "iOS Backgrounding Techniques"
description: "This document links to guides that describe various backgrounding techniques in iOS: background tasks, background transfer service, background fetch, and remote notifications."
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
---

# iOS Backgrounding Techniques

In the following sections, we will explore the following iOS features alongside the existing backgrounding options:

-  [Opportunistic Background Tasks](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) - Preserve battery life by running background tasks in opportunistic chunks when the device is awake for other processing.
-  [Background Transfer Service](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) - Reliably upload and download files regardless of network status or file size.
-  [Background Fetch](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) - Refresh an application from the background at system-determined intervals.
-  [Remote Notifications](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) - Use push notifications to trigger content updates in the background before the user opens the application, with an option to notify the user or update silently.
-  **Background UI Updates** - Prepare the application UI for the user, and update the application's snapshot, all from the background.
