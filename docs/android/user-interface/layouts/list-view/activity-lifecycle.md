---
title: "ListView and the Activity Lifecycle"
ms.prod: xamarin
ms.assetid: 40840D03-6074-30A2-74DA-3664703E3367
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
---

# ListView and the Activity Lifecycle

Activities go through certain states as your application runs, such as
starting up, running, being paused and being stopped. For more
information, and specific guidelines on handling state transitions, see the
[Activity Lifecycle Tutorial](~/android/app-fundamentals/activity-lifecycle/index.md).
It is important to understand the activity lifecycle and place your
`ListView` code in the correct locations.

All of the examples in this document perform 'setup tasks' in the
Activity's `OnCreate` method and (when required) perform
'teardown' in `OnDestroy`. The examples generally use small data
sets that do not change, so re-loading the data more frequently is
unnecessary.

However, if your data is frequently changing or uses a lot of memory it
might be appropriate to use different lifecycle methods to populate and
refresh your `ListView`. For example, if the underlying data is
constantly changing (or may be affected by updates on other activities)
then creating the adapter in `OnStart` or `OnResume` will ensure the
latest data is displayed each time the Activity is shown.

If the Adapter uses resources like memory, or a managed cursor,
remember to release those resources in the complementary method to
where they were instantiated (eg. objects created in `OnStart` can be
disposed of in `OnStop`).


## Configuration Changes

It's important to remember that configuration changes &ndash; especially
screen rotation and keyboard visibility &ndash; can cause the current
activity to be destroyed and re-created (unless you specify otherwise using 
the `ConfigurationChanges` attribute). This means that under
normal conditions, rotating a device will cause a `ListView` and
`Adapter` to be re-created and (unless you have written code in
`OnPause` and `OnResume`) the scroll position and row selection states
will be lost.

The following attribute would prevent an activity from being destroyed and
recreated as a result of configuration changes:

```csharp
[Activity(ConfigurationChanges="keyboardHidden|orientation")]
```

The Activity should then override `OnConfigurationChanged` to respond
to those changes appropriately. For more details on how to handle
configuration changes see the documentation.

