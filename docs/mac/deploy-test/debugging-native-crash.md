---
title: "Debugging a native crash in a Xamarin.Mac app"
description: "This document describes how to debug exceptions that originate in the Objective-C runtime. It discusses assertion failures, issues with callbacks, exception bubbling, and more."
ms.prod: xamarin
ms.assetid: B0C0CE31-2737-4969-8EA5-D39D3333E9C2
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 10/19/2016
---

# Debugging a native crash in a Xamarin.Mac app

## Overview

Sometimes programming errors can cause crashes in the native Objective-C runtime. Unlike C# exceptions, these don't point to a specific line in your code you can look to fix. Sometimes they can be trivial to find and fix, and other times they can be extremely difficult to track down.

Let's walk through a few real native crash examples and take a look.

## Example 1: Assertion failure

Here is the first few lines of a crash in a simple test application (this information will be in the **Application Output** Pad):

```csharp
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.364 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] *** Assertion failure in -[NSTableView _uncachedRectHeightOfRow:], /SourceCache/AppKit/AppKit-1343.13/TableView.subproj/NSTableView.m:1855
2014-10-15 16:18:02.378 NSOutlineViewHottness[79111:1304993] NSTableView variable rowHeight error: The value must be > 0 for row 0, but the delegate <NSOutlineViewHottness_HotnessViewDelegate: 0xaa01860> gave -1.000.
2014-10-15 16:18:02.381 NSOutlineViewHottness[79111:1304993] (
  0   CoreFoundation                      0x91888343 __raiseError + 195
  1   libobjc.A.dylib                     0x9a5e6a2a objc_exception_throw + 276
  2   CoreFoundation                      0x918881ca +[NSException raise:format:arguments:] + 138
  3   Foundation                          0x950742b1 -[NSAssertionHandler handleFailureInMethod:object:file:lineNumber:description:] + 118
  4   AppKit                              0x975db476 -[NSTableView _uncachedRectHeightOfRow:] + 373
  5   AppKit                              0x975db2f8 -[_NSTableRowHeightStorage _uncachedRectHeightOfRow:] + 143
  6   AppKit                              0x975db206 -[_NSTableRowHeightStorage _cacheRowHeights] + 167
  7   AppKit                              0x975db130 -[_NSTableRowHeightStorage _createRowHeightsArray] + 226
  8   AppKit                              0x975b5851 -[_NSTableRowHeightStorage _ensureRowHeights] + 73
  9   AppKit                              0x975b5790 -[_NSTableRowHeightStorage computeTableHeightForNumberOfRows:] + 89
  10  AppKit                              0x975b4c38 -[NSTableView _totalHeightOfTableView] + 220
```

The lines prefixed with numbers is the native stack trace. From that you can see the crash occurred somewhere inside `NSTableView` handling the row heights. Then `NSAssertionHandler` fires an `NSException (objc_exception_throw)` and we see the Assertion failure:

```csharp
rowHeight error: The value must be > 0 for row 0, but the delegate
<NSOutlineView_ViewDelegate: 0xaa01860> gave -1.000
```

Once you see this, it’s pretty clear that some `NSOutlineViewDelegate` method is returning a negative number. This was the problem:

```csharp
public override nfloat GetRowHeight (NSTableView tableView, nint row)
{
    return -1;
}
```

## Example 2: Callback jumped into middle of nowhere

```csharp
Stacktrace:

 at <unknown> <0xffffffff>
 at (wrapper managed-to-native) MonoMac.AppKit.NSApplication.NSApplicationMain (int,string[]) <IL 0x000a4, 0xffffffff>
 at MonoMac.AppKit.NSApplication.Main (string[]) [0x00041] in /Users/donblas/Programming/xamcore-master/src/AppKit/NSApplication.cs:107
 at NSOutlineViewHottness.MainClass.Main (string[]) [0x00007] in /Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/Main.cs:14
 at (wrapper runtime-invoke) <Module>.runtime_invoke_void_object (object,intptr,intptr,intptr) <IL 0x00050, 0xffffffff>

Native stacktrace:

Debug info from gdb:

(lldb) command source -s 0 '/tmp/mono-gdb-commands.qrHllW'
Executing commands in '/private/tmp/mono-gdb-commands.qrHllW'.
(lldb) process attach --pid 79229
Process 79229 stopped
Executable module set to "/Users/donblas/Programming/Local/NSOutlineViewHottness/NSOutlineViewHottness/bin/Debug/NSOutlineViewHottness.app/Contents/MacOS/NSOutlineViewHottness".
Architecture set to: i386-apple-macosx.
(lldb) thread list
Process 79229 stopped
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 thread #2: tid = 0x142790, 0x9af768d2 libsystem_kernel.dylib`kevent64 + 10, queue = 'com.apple.libdispatch-manager'
 thread #3: tid = 0x142792, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #4: tid = 0x142794, 0x9af6fa6a libsystem_kernel.dylib`semaphore_wait_trap + 10
 thread #5: tid = 0x142795, 0x9af75772 libsystem_kernel.dylib`__recvfrom + 10
 thread #6: tid = 0x142799, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #7: tid = 0x14279a, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #8: tid = 0x14279b, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #9: tid = 0x1427f8, 0x9af75e6e libsystem_kernel.dylib`__workq_kernreturn + 10
 thread #10: tid = 0x1427fe, 0x9af6fa2e libsystem_kernel.dylib`mach_msg_trap + 10
(lldb) thread backtrace all
* thread #1: tid = 0x142776, 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
 * frame #0: 0x9af75e1a libsystem_kernel.dylib`__wait4 + 10
   frame #1: 0x986bfb25 libsystem_c.dylib`waitpid$UNIX2003 + 48
   frame #2: 0x028ba36d libmono-2.0.dylib`mono_handle_native_sigsegv(signal=11, ctx=0x03115fe0) + 541 at mini-exceptions.c:2323
   frame #3: 0x0290a8bb libmono-2.0.dylib`mono_arch_handle_altstack_exception(sigctx=<unavailable>, fault_addr=<unavailable>, stack_ovf=0) + 155 at exceptions-x86.c:1159
   frame #4: 0x0280b4fd libmono-2.0.dylib`mono_sigsegv_signal_handler(_dummy=<unavailable>, info=<unavailable>, context=<unavailable>) + 445 at mini.c:6861
   frame #5: 0x91ef403b libsystem_platform.dylib`_sigtramp + 43
   frame #6: 0x9a5dd0bd libobjc.A.dylib`objc_msgSend + 45
   frame #7: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #8: 0x9773ba91 AppKit`-[NSApplication sendAction:to:from:] + 548
   frame #9: 0x9773b82d AppKit`-[NSControl sendAction:to:] + 102
   frame #10: 0x97934d36 AppKit`__26-[NSCell _sendActionFrom:]_block_invoke + 176
   frame #11: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #12: 0x97787975 AppKit`-[NSCell _sendActionFrom:] + 161
   frame #13: 0x979188ea AppKit`-[NSButtonCell _sendActionFrom:] + 55
   frame #14: 0x979366e6 AppKit`__48-[NSCell trackMouse:inRect:ofView:untilMouseUp:]_block_invoke965 + 43
   frame #15: 0x96bcec03 libsystem_trace.dylib`_os_activity_initiate + 89
   frame #16: 0x977a3d00 AppKit`-[NSCell trackMouse:inRect:ofView:untilMouseUp:] + 2815
   frame #17: 0x977a2df4 AppKit`-[NSButtonCell trackMouse:inRect:ofView:untilMouseUp:] + 524
   frame #18: 0x977a233b AppKit`-[NSControl mouseDown:] + 762
   frame #19: 0x97cbc112 AppKit`-[_NSThemeWidget mouseDown:] + 378
   frame #20: 0x97d36d74 AppKit`-[NSWindow _reallySendEvent:] + 12353
   frame #21: 0x977201f9 AppKit`-[NSWindow sendEvent:] + 409
   frame #22: 0x976cdc67 AppKit`-[NSApplication sendEvent:] + 4679
   frame #23: 0x9754807c AppKit`-[NSApplication run] + 1003
```

This is an issue that was much more difficult to track down. When you see `at <unknown> <0xffffffff>` or `MonoMac.ObjCRuntime.Runtime.GetNSObject (IntPtr ptr)`  at the top of the managed stack trace, it suggests we are trying to execute some managed code with an object that has been garbage collected. The native stack trace shows `trackMouse:inRect:ofView:untilMouseUp` into `NSCell _sendActionFrom` so we are somewhere handling a click event, trying to call back into C# and dying.

In general, errors like this are difficult to track down. I added `GC.Collect(2)` to a button handler to help track down this issue (forcing a garbage collection) to make the issue reproducible. After bisecting the example for a while, removing sections of code until the issue disappeared, it turned out to be [this bug](https://bugzilla.xamarin.com/show_bug.cgi?id=23378):

```csharp
mainWindowController.Window.StandardWindowButton (NSWindowButton.CloseButton).Activated += HandleActivated;
```

The `NSButton` returned by `StandardWindowButton()` was being collected even though an event was registered to it (that's the bug). When we try to call that event by clicking, if the button has been garbage collected we crash.

Although it wasn't the root cause of this particular issue, stack traces like this can also be caused by incorrect method signatures in functions `[Export]`ed to Objective-C. For example, if a method expects a parameter to be an `out string` and you type it as `string`, we can crash in the same way.

## Example 3: Callbacks and managed objects

Many Cocoa APIs involve being “called back” by the library when some event occurs, giving you a chance to respond, or when some data is needed to carry out a task. Though you may think primarily of the **Delegate** and **DataSource** patterns, there are a multitude of APIs that work this way. For example, when you override the methods of an `NSView` and then insert it into the visual tree, you expect AppKit to call you back when certain events occur.

In almost all cases, Xamarin.Mac will correctly prevent the managed object target of these callbacks from being Garbage Collected while they still could be called back. However, rarely bugs in the binding can disrupt this. When this happens, you can see unpleasant crashes similar to this:

```csharp
Thread 0 Crashed:: Dispatch queue: com.apple.main-thread

0   libsystem_kernel.dylib              0x98c2f69a __pthread_kill + 10
1   libsystem_pthread.dylib             0x90341f19 pthread_kill + 101
2   libsystem_c.dylib                   0x9453feee abort + 156
3   libmonosgen-2.0.dylib               0x020bfba5 mono_handle_native_sigsegv + 757
4   libmonosgen-2.0.dylib               0x0210b812 mono_arch_handle_altstack_exception + 162
5   libmonosgen-2.0.dylib               0x0200c55e mono_sigsegv_signal_handler + 446
6   libsystem_platform.dylib            0x9513003b _sigtramp + 43
7   ???                                 0xffffffff 0 + 4294967295
8   libmonosgen-2.0.dylib               0x0200c3a0 mono_sigill_signal_handler + 48
9   com.apple.AppKit                    0x99f76041 -[NSView setFrame:] + 448
10  com.apple.AppKit                    0x9a1fd4ea -[NSToolbarView adjustToWindow:attachedToEdge:] + 198
11  com.apple.AppKit                    0x9a1fd414 -[NSToolbar _adjustViewToWindow] + 68
12  com.apple.AppKit                    0x9a01eb0d -[NSToolbar _windowWillShowToolbar] + 79

```

This guide will help you track down bugs of this nature if they crop up, correctly report them so they can be fixed, and work around them in your code until then.

### Locating

In almost every case with bugs of this nature, the primary symptom is native crashes, normally with something similar to `mono_sigsegv_signal_handler`, or `_sigtrap` in the top frames of the stack. Cocoa is attempting to call back into your C# code, hitting a garbage collected object, and crashing. However, not every crash with these symbols is caused by a binding issue like this, you’ll need to do some additional digging to confirm this is the problem.

What makes these bugs difficult to track down is that they only occur **after** a garbage collection has disposed of the object in question. If you believe you’ve hit one of these bugs, add the following code somewhere in your startup sequence:

```csharp
new System.Threading.Thread (() =>
{
    while (true) {
         System.Threading.Thread.Sleep (1000);
         GC.Collect ();
    }
}).Start ();
```

This will force your application to run the garbage collector every second. Re-run your application and try to reproduce the bug. If you crash immediately, or consistently instead of randomly, you are on the right track.

### Reporting

The next step is to report the issue to Xamarin so the binding can be fixed for future releases. If you are a business or enterprise license holder, open a ticket at

[visualstudio.microsoft.com/vs/support/](https://visualstudio.microsoft.com/vs/support/)

Otherwise, search for an existing issue:

- Check the [Xamarin.Mac Forums](https://forums.xamarin.com/categories/xamarin-mac)
- Search the [issue repository](https://github.com/xamarin/xamarin-macios/issues)
- Before switching to GitHub issues, Xamarin issues were tracked on [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Please search there for matching issues.
- If you cannot find a matching issue, please file a new issue in the [GitHub issue repository](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub issues are all public. It’s not possible to hide comments or attachments.

Please include as much of the following as possible:

- A simple example reproducing the issue. This is **invaluable** where possible.
- The full stack trace of the crash.
- The C# code surrounding the crash.   

### Working around

Once you’ve tracked down the issue, patching the issue with a work around until the binding can be fixed can be simple. The aim is to prevent the object (**View**, **Delegate**, **DataSource**) that is incorrectly being disposed from leaving memory by keeping an open reference.

For simple cases where there is only a single instance of the object, change the code from this:

```csharp
void AddObject ()
{
    item.View = new MyView ();
    ...
}
```

To using a static variable such as this:

```csharp
static NSObject view;
...

void AddObject ()
{
    view = new MyView ();
    item.View = view;
    ...
}
```

In cases where multiple instances may be created, a static `HashSet` can be employed:

```csharp
static HashSet<NSObject> collection = new HashSet<NSObject> ();
...

void AddObject ()
{
    item.View = new MyView ();
    collection.Add (item.View );
    ...
}
```

Once the binding has been fixed and you’ve upgrade to the version of Xamarin.Mac that includes the fix, the work-around code can be removed.

## Exception bubbling and Objective-C

You should never allow a C# Exception to "escape" managed code to the calling Objective-C method. If you do, the results are undefined but generally involve crashing. In general, we do everything we can to bubble up useful information for both
native and managed crashes to help you solve your issues quickly.

Without getting too bogged down with the technical reasons why, setting up the
infrastructure to catch managed exceptions at every managed/native boundary is
non-trivially expensive and there are a _lot_ of transitions that happen in
many common operations. Many operations, specifically ones that involve the UI
thread must finish quickly or your app will stutter and have unacceptable
performance characteristics. Many of those callbacks do very simple things that
rarely have the possibility of throwing, so this overhead would both be
expensive and unnecessary in those cases.

Thus, we'd don't set up those try / catches for you. For places where you code
does non-trivial things (beyond say returning a booleans or simple math), you
can try catch yourself.
