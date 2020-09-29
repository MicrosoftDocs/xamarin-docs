---
title: "Threading in Xamarin.iOS"
description: "This document describes how to use the System.Threading APIs in a Xamarin.iOS application. It discusses The Task Parallel Library, building responsive applications, and garbage collection."
ms.prod: xamarin
ms.assetid: 50BCAF3B-1020-DDC1-0339-7028985AAC72
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
---

# Threading in Xamarin.iOS

The Xamarin.iOS runtime gives developers access to the
.NET threading APIs, both explicitly when using threads
(`System.Threading.Thread, System.Threading.ThreadPool`)
and implicitly when using the asynchronous delegate patterns or
the BeginXXX methods as well as the full range of APIs that
support the Task Parallel Library.

Xamarin strongly recommends that you use
the [Task Parallel Library](/dotnet/standard/parallel-programming/task-parallel-library-tpl) (TPL) for building applications for a few reasons:

- The default TPL scheduler will delegate Task execution to the thread pool, which in turn will dynamically grow the number of threads needed as process takes place, while avoiding a scenario where too many threads end up competing for CPU time. 
- It is easier to think about operations in terms of TPL Tasks. You can easily manipulate them, schedule them, serialize their execution or launch many in parallel with a rich set of APIs. 
- It is the foundation for programming with the new C# async language extensions. 

The thread pool will slowly grow the number of threads
as needed based on the number of CPU cores available on the
system, the system load and your application demands. You can
use this thread pool either by invoking methods in `System.Threading.ThreadPool` or by using the default `System.Threading.Tasks.TaskScheduler` (part of the *Parallel Frameworks*).

Typically developers use threads when they need to create
responsive applications and they do not want to block the main
UI run loop.

 <a name="Developing_Responsive_Applications"></a>

## Developing Responsive Applications

Access to UI elements should be limited to the same thread
that is running the main loop for your application. If you
want to make changes to the main UI from a thread, you should
queue the code by using [NSObject.InvokeOnMainThread](xref:Foundation.NSObject), like this:

```csharp
MyThreadedRoutine ()  
{  
    var result = DoComputation ();  

    // we want to update an object that is managed by the main
    // thread; To do so, we need to ensure that we only access
    // this from the main thread:

    InvokeOnMainThread (delegate {  
        label.Text = "The result is: " + result;  
    });
}
```

The above invokes the code inside the delegate in the
context of the main thread, without causing any race
conditions that could potentially crash your application.

 <a name="Threading_and_Garbage_Collection"></a>

## Threading and Garbage Collection

In the course of execution, the Objective-C runtime will
create and release objects. If objects are flagged for
"auto-release" the Objective-C runtime will release those
objects to the thread's current `NSAutoReleasePool`. Xamarin.iOS
creates one `NSAutoRelease` pool for every thread from the `System.Threading.ThreadPool` and for the main thread. This by
extension covers any threads created using the default
TaskScheduler in System.Threading.Tasks.

If you create your own threads using `System.Threading` you
do have to provide you own `NSAutoRelease` pool to prevent the
data from leaking. To do this, simply wrap your thread in the
following piece of code:

```csharp
void MyThreadStart (object arg)
{
   using (var ns = new NSAutoReleasePool ()){
      // Your code goes here.
   }
}
```

Note: Since Xamarin.iOS 5.2 you do not have to provide your own `NSAutoReleasePool` anymore as one will be provided
automatically for you.

## Related Links

- [Working with the UI Thread](~/ios/user-interface/ios-ui/ui-thread.md)