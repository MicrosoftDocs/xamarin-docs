---
title: "Android Debug Log"
ms.topic: article
ms.prod: xamarin
ms.assetid: 01A715FE-9E9D-9B85-8A59-6568D8A09CA5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
---

# Android Debug Log

## Android Debug Log Overview

One very common trick developers use to debug their applications is to
make calls to `Console.WriteLine`. However, on a mobile platform like
Android there is no console. Android devices provides a log that you
can use while writing apps. This is sometimes referred to as 'logcat'
due to the command that you type to retrieve it.

## Accessing from Visual Studio

In Visual Studio 2015 and above the Android and iOS log pads are unified.

### Visual Studio 2015 & 2017

The new Device Log tool window for Visual Studio shows the logs for 
Android and iOS devices. It can be shown by executing any of the 
following commands: 

-   **View > Other Windows > Device Log**
-   **Tools > Android > Device Log**
-   **Android toolbar > Device Log**

Once the tool window is displayed, the physical device can be selected 
from the devices combo box. When the device is selected, it 
automatically starts to add log entries from a running app in the 
table. Switching between devices will stop and start the device 
logging.In order for the devices to appear in the combobox, an Android 
project must be loaded. If the device does not appear in the combobox, 
check first if it's available in the Start Debug drop down. 

This tool window provides: a table of log entries, a combo box for 
device selection, a way to clear log entries, a search box, and 
play/stop/pause buttons. 


<a name="Accessing_from_the_Command_Line" />

## Accessing from the Command Line

Another option to view the debug log is via the command line. Open a 
console window and navigate to the Android SDK platform-tools folder 
(like **C:\android-sdk-windows\platform-tools**). 

If only one device is attached, the log can be viewed with:

```shell
$ adb logcat
```

If more than one device is attached, then the device must be 
identified. For example `adb -d logcat` shows the log of the only 
physical device connected, while `adb -e logcat` shows the log of the 
only emulator running. 

More commands can be found by just running **adb**.

<a name="Writing_to_the_Debug_Log" />


## Writing to the Debug Log

Messages can be written to the Debug Log using methods on the 
[Android.Util.Log](https://developer.xamarin.com/api/type/Android.Util.Log/) class: 

```csharp
string tag = "myapp";

Log.Info (tag, "this is an info message");
Log.Warn (tag, "this is a warning message");
Log.Error (tag, "this is an error message");
```

This produces:

```shell
I/myapp   (11103): this is an info message
W/myapp   (11103): this is a warning message
E/myapp   (11103): this is an error message
```

<a name="Interesting_Messages" />

## Interesting Messages

When reading the log, and especially when providing log snippets to others
(as providing the full log file is (1) overkill, and (2) unreadable),
the *most important* line to start with is a line resembling:

```shell
I/ActivityManager(12944): Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] flg=0x10200000 cmp=GcTest.GcTest/gctest.Activity1 } from pid 24175
```

Specifically, a line matching the regular expression:

```shell
^I.*ActivityManager.*Starting: Intent
```

and containing the name of the application package. This is the line 
which corresponds to the start of an activity, and *most* (but not all) 
of the following messages should relate to the application. 

In particular, every message contains the process identifier (pid) of 
the process generating the message. In the above `ActivityManager` 
message, process `12944` generated the message. To determine which 
process is the process of the application being debugged, look for the 
mono.MonoRuntimeProvider message: 

```shell
I/ActivityThread(  602): Pub TouchTest.TouchTest.__mono_init__: mono.MonoRuntimeProvider
```

This message comes from the process that was started, and all following 
messages that contain the same pid come from the same process. 
