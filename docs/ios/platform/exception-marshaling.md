---
title: "Exception Marshaling in Xamarin.iOS"
description: "This document describes how to work with native and managed exceptions in a Xamarin.iOS app. It discusses problems that can occur and a solution for these problems."
ms.prod: xamarin
ms.assetid: BE4EE969-C075-4B9A-8465-E393556D8D90
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/05/2017
---

# Exception Marshaling in Xamarin.iOS

_Xamarin.iOS contains new events to help respond to exceptions, particularly in native code._

Both managed code and Objective-C have support for runtime exceptions
(try/catch/finally clauses).

However, their implementations are different, which means that the
runtime libraries (the Mono runtime and the Objective-C runtime libraries) 
have problems when they have to handle exceptions and then run code written in
other languages.

This document explains the problems that can occur, and the possible
solutions.

It also includes a sample project, [Exception Marshaling](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling),
which can be used to test different scenarios and their solutions.

## Problem

The problem occurs when an exception is thrown, and during stack unwinding a
frame is encountered which does not match the type of exception that was
thrown.

A typical example of this for Xamarin.iOS or Xamarin.Mac is when a native API
throws an Objective-C exception, and then that Objective-C exception must
somehow be handled when the stack unwinding process reaches a managed frame.

The default action is to do nothing. For the sample above, this means letting
the Objective-C runtime unwind managed frames. This is problematic, because
the Objective-C runtime does not know how to unwind managed frames; for
example it won't execute any `catch` or `finally` clauses in that frame.

### Broken code

Consider the following code example:

``` csharp
var dict = new NSMutableDictionary ();
dict.LowlevelSetObject (IntPtr.Zero, IntPtr.Zero); 
```

This will throw an Objective-C NSInvalidArgumentException in native code:

```
NSInvalidArgumentException *** setObjectForKey: key cannot be nil
```

And the stack trace will be something like this:

```
0   CoreFoundation          __exceptionPreprocess + 194
1   libobjc.A.dylib         objc_exception_throw + 52
2   CoreFoundation          -[__NSDictionaryM setObject:forKey:] + 1015
3   libobjc.A.dylib         objc_msgSend + 102
4   TestApp                 ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
5   TestApp                 Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr)
6   TestApp                 ExceptionMarshaling.Exceptions.ThrowObjectiveCException ()
```

Frames 0-3 are native frames, and the stack unwinder in the Objective-C
runtime _can_ unwind those frames. In particular, it will execute any
Objective-C `@catch` or `@finally` clauses.

However, the Objective-C stack unwinder is _not_ capable of properly unwinding the
managed frames (frames 4-6), in that the frames will be unwound, but
managed exception logic will not be executed.

Which means that it's usually not possible to catch these exceptions in the following manner:

```csharp
try {
    var dict = new NSMutableDictionary ();
    dict.LowLevelSetObject (IntPtr.Zero, IntPtr.Zero);
} catch (Exception ex) {
    Console.WriteLine (ex);
} finally {
    Console.WriteLine ("finally");
}
```

This is because the Objective-C stack unwinder does not know about the managed `catch`
clause, and neither will the `finally` clause be executed.

When the above code sample _is_ effective, it is because Objective-C has a method of being notified of unhandled
Objective-C exceptions, [`NSSetUncaughtExceptionHandler`][2], which Xamarin.iOS
and Xamarin.Mac use, and at that point tries to convert any Objective-C
exceptions to managed exceptions.

## Scenarios

### Scenario 1 - catching Objective-C exceptions with a managed catch handler

In the following scenario, it is possible to catch Objective-C exceptions using
managed `catch` handlers:

1. An Objective-C exception is thrown.
2. The Objective-C runtime walks the stack (but does not unwind it), looking
   for a native `@catch` handler that can handle the exception.
3. The Objective-C runtime doesn't find any `@catch` handlers, calls
   `NSGetUncaughtExceptionHandler`, and invokes the handler installed by
   Xamarin.iOS/Xamarin.Mac.
4. Xamarin.iOS/Xamarin.Mac's handler will convert the Objective-C exception
   into a managed exception, and throw it. Since the Objective-C runtime
   didn't unwind the stack (only walked it), the current frame is the same
   one where the Objective-C exception was thrown.

Another problem occurs here, because the Mono runtime does not know how
to unwind Objective-C frames properly.

When Xamarin.iOS' uncaught Objective-C exception callback is called, the stack
is like this:

```
 0 libxamarin-debug.dylib   exception_handler(exc=name: "NSInvalidArgumentException" - reason: "*** setObjectForKey: key cannot be nil")
 1 CoreFoundation           __handleUncaughtException + 809
 2 libobjc.A.dylib          _objc_terminate() + 100
 3 libc++abi.dylib          std::__terminate(void (*)()) + 14
 4 libc++abi.dylib          __cxa_throw + 122
 5 libobjc.A.dylib          objc_exception_throw + 337
 6 CoreFoundation           -[__NSDictionaryM setObject:forKey:] + 1015
 7 libxamarin-debug.dylib   xamarin_dyn_objc_msgSend + 102
 8 TestApp                  ObjCRuntime.Messaging.void_objc_msgSend_IntPtr_IntPtr (intptr,intptr,intptr,intptr)
 9 TestApp                  Foundation.NSMutableDictionary.LowlevelSetObject (intptr,intptr) [0x00000]
10 TestApp                  ExceptionMarshaling.Exceptions.ThrowObjectiveCException () [0x00013]
```

Here, the only managed frames are frames 8-10, but the managed exception is
thrown in frame 0. This means that the Mono runtime must unwind the native
frames 0-7, which causes a problem equivalent to the problem discussed above: 
although the Mono runtime will unwind the native frames, it won't execute any Objective-C `@catch` or
`@finally` clauses.

Code example:

```objc
-(id) setObject: (id) object forKey: (id) key
{
    @try {
        if (key == nil)
            [NSException raise: @"NSInvalidArgumentException"];
    } @finally {
        NSLog (@"This will not be executed");
    }
}
```

And the `@finally` clause will not be executed because the Mono runtime that
unwinds this frame does not know about it.

A variation of this is to throw a managed exception in managed code, and then
unwinding through native frames to get to the first managed `catch`
clause:

```csharp
class AppDelegate : UIApplicationDelegate {
    public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
    {
        throw new Exception ("An exception");
    }
    static void Main (string [] args)
    {
        try {
            UIApplication.Main (args, null, typeof (AppDelegate));
        } catch (Exception ex) {
            Console.WriteLine ("Managed exception caught.");
        }
    }
}
```

The managed `UIApplication:Main` method will call the native
`UIApplicationMain` method, and then iOS will do a lot of native code
execution before eventually calling the managed
`AppDelegate:FinishedLaunching` method, with still a lot of native frames on
the stack when the managed exception is thrown:

```
 0: TestApp                 ExceptionMarshaling.IOS.AppDelegate:FinishedLaunching (UIKit.UIApplication,Foundation.NSDictionary)
 1: TestApp                 (wrapper runtime-invoke) <Module>:runtime_invoke_bool__this___object_object (object,intptr,intptr,intptr) 
 2: libmonosgen-2.0.dylib   mono_jit_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
 3: libmonosgen-2.0.dylib   do_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>, error=<unavailable>)
 4: libmonosgen-2.0.dylib   mono_runtime_invoke [inlined] mono_runtime_invoke_checked(method=<unavailable>, obj=<unavailable>, params=<unavailable>, error=0xbff45758)
 5: libmonosgen-2.0.dylib   mono_runtime_invoke(method=<unavailable>, obj=<unavailable>, params=<unavailable>, exc=<unavailable>)
 6: libxamarin-debug.dylib  xamarin_invoke_trampoline(type=<unavailable>, self=<unavailable>, sel="application:didFinishLaunchingWithOptions:", iterator=<unavailable>), context=<unavailable>)
 7: libxamarin-debug.dylib  xamarin_arch_trampoline(state=0xbff45ad4)
 8: libxamarin-debug.dylib  xamarin_i386_common_trampoline
 9: UIKit                   -[UIApplication _handleDelegateCallbacksWithOptions:isSuspended:restoreState:]
10: UIKit                   -[UIApplication _callInitializationDelegatesForMainScene:transitionContext:]
11: UIKit                   -[UIApplication _runWithMainScene:transitionContext:completion:]
12: UIKit                   __84-[UIApplication _handleApplicationActivationWithScene:transitionContext:completion:]_block_invoke.3124
13: UIKit                   -[UIApplication workspaceDidEndTransaction:]
14: FrontBoardServices      __37-[FBSWorkspace clientEndTransaction:]_block_invoke_2
15: FrontBoardServices      __40-[FBSWorkspace _performDelegateCallOut:]_block_invoke
16: FrontBoardServices      __FBSSERIALQUEUE_IS_CALLING_OUT_TO_A_BLOCK__
17: FrontBoardServices      -[FBSSerialQueue _performNext]
18: FrontBoardServices      -[FBSSerialQueue _performNextFromRunLoopSource]
19: FrontBoardServices      FBSSerialQueueRunLoopSourceHandler
20: CoreFoundation          __CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__
21: CoreFoundation          __CFRunLoopDoSources0
22: CoreFoundation          __CFRunLoopRun
23: CoreFoundation          CFRunLoopRunSpecific
24: CoreFoundation          CFRunLoopRunInMode
25: UIKit                   -[UIApplication _run]
26: UIKit                   UIApplicationMain
27: TestApp                 (wrapper managed-to-native) UIKit.UIApplication:UIApplicationMain (int,string[],intptr,intptr)
28: TestApp                 UIKit.UIApplication:Main (string[],intptr,intptr)
29: TestApp                 UIKit.UIApplication:Main (string[],string,string)
30: TestApp                 ExceptionMarshaling.IOS.Application:Main (string[])
```

Frames 0-1 and 27-30 are managed, while all those in between are native. If
Mono unwinds through these frames, no Objective-C `@catch` or `@finally`
clauses will be executed.

### Scenario 2 - not able to catch Objective-C exceptions

In the following scenario, it is _not_ possible to catch Objective-C exceptions
using managed `catch` handlers because the Objective-C exception was handled
in another way:

1. An Objective-C exception is thrown.
2. The Objective-C runtime walks the stack (but does not unwind it), looking
   for a native `@catch` handler that can handle the exception.
3. The Objective-C runtime finds a `@catch` handler, unwinds the stack, and
   starts executing the `@catch` handler.

This scenario is commonly found in Xamarin.iOS apps, because on the main thread there
is usually code like this:

``` objective-c
void UIApplicationMain ()
{
    @try {
        while (true) {
            ExecuteRunLoop ();
        }
    } @catch (NSException *ex) {
        NSLog (@"An unhandled exception occured: %@", exc);
        abort ();
    }
}

```

This means that on the main thread there's never really an unhandled
Objective-C exception, and thus our callback that converts Objective-C
exceptions to managed exceptions is never called.

This is also quite common when debugging Xamarin.Mac apps on an earlier
macOS version than Xamarin.Mac supports because inspecting most UI objects in
the debugger will try to fetch properties that correspond to selectors that
don't exist on the executing platform (because Xamarin.Mac includes support
for a higher macOS version). Calling such selectors will throw an
`NSInvalidArgumentException` ("Unrecognized selector sent to ..."), which
eventually causes the process to crash.

To summarize, having either the Objective-C runtime or the Mono runtime unwind
frames that they are not programmed to handle can lead to undefined behaviors, 
such as crashes, memory leaks, and other types of unpredictable (mis)behaviors.

## Solution

In Xamarin.iOS 10 and Xamarin.Mac 2.10, we've added support for catching both
managed and Objective-C exceptions on any managed-native boundary, and for converting
that exception to the other type.

In pseudo-code, it looks something like this:

``` csharp
[DllImport ("libobjc.dylib")]
static extern void objc_msgSend (IntPtr handle, IntPtr selector);

static void DoSomething (NSObject obj)
{
    objc_msgSend (obj.Handle, Selector.GetHandle ("doSomething"));
}
```

The P/Invoke to objc_msgSend is intercepted, and this is called instead:

``` objective-c
void
xamarin_dyn_objc_msgSend (id obj, SEL sel)
{
    @try {
        objc_msgSend (obj, sel);
    } @catch (NSException *ex) {
        convert_to_and_throw_managed_exception (ex);
    }
}
```

And something similar is done for the reverse case (marshaling managed
exceptions to Objective-C exceptions).

Catching exceptions on the managed-native boundary is not cost-free, so it's
not always enabled by default:

- Xamarin.iOS/tvOS: interception of Objective-C exceptions is enabled in the
  simulator.
- Xamarin.watchOS: interception is enforced in all cases, because letting the
  Objective-C runtime unwind managed frames will confuse the garbage
  collector, and either make it hang or crash.
- Xamarin.Mac: interception of Objective-C exceptions is enabled for debug
  builds.

The [Build-time flags](#build_time_flags) section explains how to enable interception when it's
not enabled by default.

## Events

There are two new events that are raised once an exception is intercepted:
`Runtime.MarshalManagedException` and `Runtime.MarshalObjectiveCException`.

Both events are passed an `EventArgs` object that contains the original
exception that was thrown (the `Exception` property), and an `ExceptionMode`
property to define how the exception should be marshaled.

The `ExceptionMode` property can be changed in the event handler to change the
behavior according to any custom processing done in the handler. One example
would be to abort the process if a certain exception occurs.

Changing the `ExceptionMode` property applies to the single event, it does
not affect any exceptions intercepted in the future.

The following modes are available:

- `Default`: The default varies by platform. It is
  `ThrowObjectiveCException` if the GC is in cooperative mode (watchOS), and
  `UnwindNativeCode` otherwise (iOS / watchOS / macOS). The default may change
  in the future.
- `UnwindNativeCode`: This is the previous (undefined) behavior. This is
  not available when using the GC in cooperative mode (which is the only
  option on watchOS; thus, this is not a valid option on watchOS), but it's the
  default option for all other platforms.
- `ThrowObjectiveCException`: Convert the managed exception into an Objective-C
  exception and throw the Objective-C exception. This is the default on
  watchOS.
- `Abort`: Abort the process.
- `Disable`: Disables the exception interception, so it doesn't make sense to set this value in the event handler,
  but once the event is raised it's too late to disable it. In any case, if set, it will behave as
  `UnwindNativeCode`.

For marshaling Objective-C exceptions to managed code, the following modes are
available:

- `Default`: The default varies by platform. It is
  `ThrowManagedException` if the GC is in cooperative mode (watchOS), and
  `UnwindManagedCode` otherwise (iOS / tvOS / macOS). The default may change
  in the future.
- `UnwindManagedCode`: This is the previous (undefined) behavior. This
  is not available when using the GC in cooperative mode (which is the only
  valid GC mode on watchOS; thus this is not a valid option on watchOS), but
  it's the default for all other platforms.
- `ThrowManagedException`: Convert the Objective-C exception to a managed
  exception and throw the managed exception. This is the default on watchOS.
- `Abort`: Abort the process.
- `Disable`:Disables the exception interception, so it doesn't make sense to set this value in the event handler, 
  but once the event is raised, it's too late to disable it. In any case if set, it will abort the process.

So, to see every time an exception is marshaled, you can do this:

``` csharp
Runtime.MarshalManagedException += (object sender, MarshalManagedExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling managed exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
    
};
Runtime.MarshalObjectiveCException += (object sender, MarshalObjectiveCExceptionEventArgs args) =>
{
    Console.WriteLine ("Marshaling Objective-C exception");
    Console.WriteLine ("    Exception: {0}", args.Exception);
    Console.WriteLine ("    Mode: {0}", args.ExceptionMode);
};
```

<a name="build_time_flags"></a>

## Build-Time Flags

It's possible to pass the following options to **mtouch** (for Xamarin.iOS apps)
and **mmp** (for Xamarin.Mac apps), which will determine if exception interception
is enabled, and set the default action that should occur:

- `--marshal-managed-exceptions=`
  - `default`
  - `unwindnativecode`
  - `throwobjectivecexception`
  - `abort`
  - `disable`

- `--marshal-objectivec-exceptions=`
  - `default`
  - `unwindmanagedcode`
  - `throwmanagedexception`
  - `abort`
  - `disable`

Except for `disable`, these values are identical to the `ExceptionMode`
values that are passed to the `MarshalManagedException` and
`MarshalObjectiveCException` events.

The `disable` option will _mostly_ disable interception, except we'll still
intercept exceptions when it does not add any execution overhead. The
marshaling events are still raised for these exceptions, with the default mode
being the default mode for the executing platform.

## Limitations

We only intercept P/Invokes to the `objc_msgSend` family of functions when
trying to catch Objective-C exceptions. This means that a P/Invoke to another
C function, which then throws any Objective-C exceptions, will still run into
the old and undefined behavior (this may be improved in the future).

[2]: https://developer.apple.com/reference/foundation/1409609-nssetuncaughtexceptionhandler?language=objc

## Related Links

- [Exception Marshaling (sample)](https://github.com/xamarin/mac-ios-samples/tree/master/ExceptionMarshaling)
