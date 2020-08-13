---
title: "Garbage Collection"
ms.prod: xamarin
ms.assetid: 298139E2-194F-4A58-BC2D-1D22231066C4
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/15/2018
---

# Garbage Collection

Xamarin.Android uses Mono's 
[Simple Generational garbage collector](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/). 
This is a mark-and-sweep garbage collector with two generations and a *large 
object space*, with two kinds of collections: 

- Minor collections (collects Gen0 heap) 
- Major collections (collects Gen1 and large object space heaps). 

> [!NOTE]
> In the absence of an explicit collection via
[GC.Collect()](xref:System.GC.Collect)
collections are *on demand*, based upon heap allocations. *This is not
a reference counting system*; objects *will not be collected as soon as
there are no outstanding references*, or when a scope has exited. The
GC will run when the minor heap has run out of memory for new
allocations. If there are no allocations, it will not run.

Minor collections are cheap and frequent, and are used to collect 
recently allocated and dead objects. Minor collections are performed 
after every few MB of allocated objects. Minor collections may be 
manually performed by calling 
[GC.Collect (0)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 

Major collections are expensive and less frequent, and are used to 
reclaim all dead objects. Major collections are performed once memory 
is exhausted for the current heap size (before resizing the heap). 
Major collections may be manually performed by calling 
[GC.Collect ()](xref:System.GC.Collect) 
or by calling 
[GC.Collect (int)](/dotnet/api/system.gc.collect#System_GC_Collect_System_Int32_) 
with the argument 
[GC.MaxGeneration](xref:System.GC.MaxGeneration). 

## Cross-VM Object Collections

There are three categories of object types.

- **Managed objects**: types which do *not* inherit from
    [Java.Lang.Object](xref:Java.Lang.Object) 
    , e.g. 
    [System.String](xref:System.String). 
    These are collected normally by the GC. 

- **Java objects**: Java types which are present within the Android 
    runtime VM but not exposed to the Mono VM. These are boring, and 
    won't be discussed further. These are collected normally by the 
    Android runtime VM. 

- **Peer objects**: types which implement
    [IJavaObject](xref:Android.Runtime.IJavaObject) 
    , e.g. all 
    [Java.Lang.Object](xref:Java.Lang.Object) 
    and 
    [Java.Lang.Throwable](xref:Java.Lang.Throwable) 
    subclasses. Instances of these types have two "halfs" a *managed 
    peer* and a *native peer*. The managed peer is an instance of the 
    C# class. The native peer is an instance of a Java class within the 
    Android runtime VM, and the C# 
    [IJavaObject.Handle](xref:Android.Runtime.IJavaObject.Handle) 
    property contains a JNI global reference to the native peer. 

There are two types of native peers:

- **Framework peers** : "Normal" Java types which know nothing of
    Xamarin.Android, e.g.
    [android.content.Context](xref:Android.Content.Context).

- **User peers** :
    [Android Callable Wrappers](~/android/platform/java-integration/working-with-jni.md)
    which are generated at build time for each Java.Lang.Object
    subclass present within the application.

As there are two VMs within a Xamarin.Android process, there are two
types of garbage collections:

- Android runtime collections 
- Mono collections 

Android runtime collections operate normally, but with a caveat: a JNI
global reference is treated as a GC root. Consequently, if there is a
JNI global reference holding onto an Android runtime VM object, the
object *cannot* be collected, even if it's otherwise eligible for
collection.

Mono collections are where the fun happens. Managed objects are collected
normally. Peer objects are collected by performing the following process:

1. All Peer objects eligible for Mono collection have their JNI global 
    reference replaced with a JNI weak global reference. 

2. An Android runtime VM GC is invoked. Any Native peer instance may be 
    collected. 

3. The JNI weak global references created in (1) are checked. If the 
    weak reference has been collected, then the Peer object is 
    collected. If the weak reference has *not* been collected, then the 
    weak reference is replaced with a JNI global reference and the Peer 
    object is not collected. Note: on API 14+, this means that the 
    value returned from `IJavaObject.Handle` may change after a GC. 

The end result of all this is that an instance of a Peer object will
live as long as it is referenced by either managed code (e.g. stored in
a `static` variable) or referenced by Java code. Furthermore, the
lifetime of Native peers will be extended beyond what they would
otherwise live, as the Native peer won't be collectible until both the
Native peer and the Managed peer are collectible.

## Object Cycles

Peer objects are logically present within both the Android runtime and Mono 
VM's. For example, an 
[Android.App.Activity](xref:Android.App.Activity) 
managed peer instance will have a corresponding 
[android.app.Activity](https://developer.android.com/reference/android/app/Activity.html) 
framework peer Java instance. All objects that inherit from 
[Java.Lang.Object](xref:Java.Lang.Object) 
can be expected to have representations within both VMs. 

All objects that have representation in both VMs will have lifetimes 
which are extended compared to objects which are present only within a 
single VM (such as a 
[`System.Collections.Generic.List<int>`](xref:System.Collections.Generic.List%601)). 
Calling 
[GC.Collect](xref:System.GC.Collect) 
won't necessarily collect these objects, as the Xamarin.Android GC 
needs to ensure that the object isn't referenced by either VM before 
collecting it. 

To shorten object lifetime, 
[Java.Lang.Object.Dispose()](xref:Java.Lang.Object.Dispose) 
should be invoked. This will manually "sever" the connection on the 
object between the two VMs by freeing the global reference, thus 
allowing the objects to be collected faster. 

## Automatic Collections

Beginning with
[Release 4.1.0](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/mono_for_android_4/mono_for_android_4.1.0/index.md), 
Xamarin.Android automatically performs a full GC when a gref 
threshold is crossed. This threshold is 90% of the known maximum grefs 
for the platform: 1800 grefs on the emulator (2000 max), and 46800 
grefs on hardware (maximum 52000). *Note:* Xamarin.Android only counts 
the grefs created by 
[Android.Runtime.JNIEnv](xref:Android.Runtime.JNIEnv), 
and will not know about any other grefs created in the process. This is 
a heuristic *only*. 

When an automatic collection is performed, a message similar to the following
will be printed to the debug log:

```shell
I/monodroid-gc(PID): 46800 outstanding GREFs. Performing a full GC!
```

The occurrence of this is non-deterministic, and may happen at 
inopportune times (e.g. in the middle of graphics rendering). If you 
see this message, you may want to perform an explicit collection 
elsewhere, or you may want to try to 
[reduce the lifetime of peer objects](#Helping_the_GC). 

## GC Bridge Options

Xamarin.Android offers transparent memory management with Android and 
the Android runtime. It is implemented as an extension to the Mono garbage collector 
called the *GC Bridge*. 

The GC Bridge works during a Mono garbage collection and figures out 
which peer objects needs their "liveness" verified with the Android 
runtime heap. The GC Bridge makes this determination by doing 
the following steps (in order):

1. Induce the mono reference graph of unreachable peer objects into 
    the Java objects they represent. 

2. Perform a Java GC.

3. Verify which objects are really dead. 

This complicated process is what enables subclasses of 
`Java.Lang.Object` to freely reference any objects; it removes any 
restrictions on which Java objects can be bound to C#. Because of this 
complexity, the bridge process can be very expensive and it can cause 
noticeable pauses in an application. If the application is experiencing 
significant pauses, it's worth investigating one of the following three 
GC Bridge implementations: 

- **Tarjan** - A completely new design of the GC Bridge 
    based on [Robert Tarjan's algorithm and backwards reference propagation](https://en.wikipedia.org/wiki/Tarjan's_strongly_connected_components_algorithm).
    It has the best performance under our simulated workloads, but it also has 
    the larger share of experimental code. 

- **New** - A major overhaul of the original code, fixing two 
    instances of quadratic behavior but keeping the core algorithm 
    (based on 
    [Kosaraju's 
    algorithm](https://en.wikipedia.org/wiki/Kosaraju's_algorithm) for 
    finding strongly connected components). 

- **Old** - The original implementation (considered the most 
    stable of the three). This is the bridge that an application should 
    use if the `GC_BRIDGE` pauses are acceptable. 

The only way to figure out which GC Bridge works best is by experimenting 
in an application and analyzing the output. There are two ways to 
collect the data for benchmarking: 

- **Enable logging** - Enable logging (as describe in the 
    [Configuration](~/android/internals/garbage-collection.md) 
    section) for each GC Bridge option, then capture and compare the 
    log outputs from each setting. Inspect the `GC` messages for each 
    option; in particular, the `GC_BRIDGE` messages. Pauses up to 150ms 
    for non-interactive applications are tolerable, but pauses above 60ms 
    for very interactive applications (such as games) are a problem. 

- **Enable bridge accounting** - Bridge accounting will display the 
    average cost of the objects pointed by each object involved in the 
    bridge process. Sorting this information by size will provide hints 
    as to what is holding the largest amount of extra objects. 

The default setting is **Tarjan**. If you find a regression, you 
may find it necessary to set this option to **Old**. Also, you may 
choose to use the more stable **Old** option if **Tarjan** does not 
produce an improvement in performance.

To specify which `GC_BRIDGE` option an application should use, pass 
`bridge-implementation=old`, `bridge-implementation=new` or 
`bridge-implementation=tarjan` to the `MONO_GC_PARAMS` environment 
variable. This is accomplished by adding a new file to your project 
with a **Build action** of `AndroidEnvironment`. For example: 

```shell
MONO_GC_PARAMS=bridge-implementation=tarjan
```

For more information, see [Configuration](#configuration).

<a name="Helping_the_GC"></a>

## Helping the GC

There are multiple ways to help the GC to reduce memory use and collection
times.

### Disposing of Peer instances

The GC has an incomplete view of the process and may not run when 
memory is low because the GC doesn't know that memory is low. 

For example, an instance of a 
[Java.Lang.Object](xref:Java.Lang.Object) 
type or derived type is at least 20 bytes in size (subject to change 
without notice, etc., etc.). 
[Managed Callable 
Wrappers](~/android/internals/architecture.md) do not add 
additional instance members, so when you have a 
[Android.Graphics.Bitmap](xref:Android.Graphics.Bitmap) 
instance that refers to a 10MB blob of memory, Xamarin.Android's GC 
won't know that &ndash; the GC will see a 20-byte object and will be unable to determine 
that it's linked to Android runtime-allocated objects that's keeping 10MB of 
memory alive. 

It is frequently necessary to help the GC. Unfortunately, 
*GC.AddMemoryPressure()* and *GC.RemoveMemoryPressure()* are not 
supported, so if you *know* that you just freed a large Java-allocated 
object graph you may need to manually call 
[GC.Collect()](xref:System.GC.Collect) 
to prompt a GC to release the Java-side memory, or you can explicitly 
dispose of *Java.Lang.Object* subclasses, breaking the mapping between 
the managed callable wrapper and the Java instance. For example, see 
[Bug 1084](https://bugzilla.xamarin.com/show_bug.cgi?id=1084#c6). 

> [!NOTE]
> You must be *extremely* careful when disposing of
`Java.Lang.Object` subclass instances.

To minimize the possibility of memory corruption, observe the following
guidelines when calling `Dispose()`.

#### Sharing Between Multiple Threads

If the *Java or managed* instance may be shared between multiple 
threads, *it should not be `Dispose()`d*, **ever**. For example, 
[`Typeface.Create()`](xref:Android.Graphics.Typeface.Create*) 
may return a *cached instance*. If multiple threads provide the same 
arguments, they will obtain the *same* instance. Consequently, 
`Dispose()`ing of the `Typeface` instance from one thread may 
invalidate other threads, which can result in `ArgumentException`s from 
`JNIEnv.CallVoidMethod()` (among others) because the instance was 
disposed from another thread. 

#### Disposing Bound Java Types

If the instance is of a bound Java type, the instance can be disposed 
of *as long as* the instance won't be reused from managed code *and* 
the Java instance can't be shared amongst threads (see previous 
`Typeface.Create()` discussion). (Making this determination may be 
difficult.) The next time the Java instance enters managed code, a 
*new* wrapper will be created for it. 

This is frequently useful when it comes to Drawables and other resource-heavy
instances:

```csharp
using (var d = Drawable.CreateFromPath ("path/to/filename"))
    imageView.SetImageDrawable (d);
```

The above is safe because the Peer that 
[Drawable.CreateFromPath()](xref:Android.Graphics.Drawables.Drawable.CreateFromPath*) 
returns will refer to a Framework peer, *not* a User peer. The 
`Dispose()` call at the end of the `using` block will break the 
relationship between the managed 
[Drawable](xref:Android.Graphics.Drawables.Drawable) 
and framework 
[Drawable](https://developer.android.com/reference/android/graphics/drawable/Drawable.html) 
instances, allowing the Java instance to be collected as soon as the 
Android runtime needs to. This would *not* be safe if Peer instance 
referred to a User peer; here we're using "external" information to 
*know* that the `Drawable` cannot refer to a User peer, and thus the 
`Dispose()` call is safe. 

#### Disposing Other Types 

If the instance refers to a type that isn't a binding of a Java type 
(such as a custom `Activity`), **DO NOT** call `Dispose()` unless you 
*know* that no Java code will call overridden methods on that instance. 
Failure to do so results in 
[`NotSupportedException`s](~/android/internals/architecture.md#Premature_Dispose_Calls). 

For example, if you have a custom click listener:

```csharp
partial class MyClickListener : Java.Lang.Object, View.IOnClickListener {
    // ...
}
```

You *should not* dispose of this instance, as Java will attempt to invoke
methods on it in the future:

```csharp
// BAD CODE; DO NOT USE
Button b = FindViewById<Button> (Resource.Id.myButton);
using (var listener = new MyClickListener ())
    b.SetOnClickListener (listener);
```

#### Using Explicit Checks to Avoid Exceptions

If you've implemented a 
[Java.Lang.Object.Dispose](xref:Java.Lang.Object.Dispose*) 
overload method, avoid touching objects that involve JNI. Doing so 
may create a *double-dispose* situation that makes it possible for 
your code to (fatally) attempt to access an underlying Java object that has 
already been garbage-collected. Doing so produces an exception 
similar to the following: 

```shell
System.ArgumentException: 'jobject' must not be IntPtr.Zero.
Parameter name: jobject
at Android.Runtime.JNIEnv.CallVoidMethod
```

This situation often occurs when the first dispose of an object 
causes a member to become null, and then a subsequent 
access attempt on this null member causes an exception to be 
thrown. Specifically, the object's `Handle` (which links a managed 
instance to its underlying Java instance) is invalidated on the 
first dispose, but managed code still attempts to access this 
underlying Java instance even though it is no longer available (see 
[Managed Callable Wrappers](~/android/internals/architecture.md#Managed_Callable_Wrappers) 
for more about the mapping between Java instances and managed 
instances). 

A good way to prevent this exception is to explicitly 
verify in your `Dispose` method that the mapping between the 
managed instance and the underlying Java instance is still valid; 
that is, check to see if the object's `Handle` is null 
(`IntPtr.Zero`) before accessing its members. For example, the 
following `Dispose` method accesses a `childViews` object: 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);
        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```

If an initial dispose pass causes `childViews` to have an invalid 
`Handle`, the `for` loop access will throw an `ArgumentException`. By 
adding an explicit `Handle` null check before the first `childViews` 
access, the following `Dispose` method prevents the exception from 
occurring: 

```csharp
class MyClass : Java.Lang.Object, ISomeInterface 
{
    protected override void Dispose (bool disposing)
    {
        base.Dispose (disposing);

        // Check for a null handle:
        if (this.childViews.Handle == IntPtr.Zero)
            return;

        for (int i = 0; i < this.childViews.Count; ++i)
        {
            // ...
        }
    }
}
```

### Reduce Referenced Instances

Whenever an instance of a `Java.Lang.Object` type or subclass is 
scanned during the GC, the entire *object graph* that the instance 
refers to must also be scanned. The object graph is the set of object 
instances that the "root instance" refers to, *plus* everything 
referenced by what the root instance refers to, recursively. 

Consider the following class:

```csharp
class BadActivity : Activity {

    private List<string> strings;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```

When `BadActivity` is constructed, the object graph will contain 10004 
instances (1x `BadActivity`, 1x `strings`, 1x `string[]` held by 
`strings`, 10000x string instances), *all* of which will need to be 
scanned whenever the `BadActivity` instance is scanned. 

This can have detrimental impacts on your collection times, resulting 
in increased GC pause times. 

You can help the GC by *reducing* the size of object graphs which are 
rooted by User peer instances. In the above example, this can be done 
by moving `BadActivity.strings` into a separate class which doesn't 
inherit from Java.Lang.Object: 

```csharp
class HiddenReference<T> {

    static Dictionary<int, T> table = new Dictionary<int, T> ();
    static int idgen = 0;

    int id;

    public HiddenReference ()
    {
        lock (table) {
            id = idgen ++;
        }
    }

    ~HiddenReference ()
    {
        lock (table) {
            table.Remove (id);
        }
    }

    public T Value {
        get { lock (table) { return table [id]; } }
        set { lock (table) { table [id] = value; } }
    }
}

class BetterActivity : Activity {

    HiddenReference<List<string>> strings = new HiddenReference<List<string>>();

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        strings.Value = new List<string> (
                Enumerable.Range (0, 10000)
                .Select(v => new string ('x', v % 1000)));
    }
}
```

## Minor Collections

Minor collections may be manually performed by calling 
[GC.Collect(0)](xref:System.GC.Collect). 
Minor collections are cheap (when compared to major collections), but 
do have a significant fixed cost, so you don't want to trigger them too 
often, and should have a pause time of a few milliseconds. 

If your application has a "duty cycle" in which the same thing is done 
over and over, it may be advisable to manually perform a minor 
collection once the duty cycle has ended. Example duty cycles include: 

- The rendering cycle of a single game frame.
- The whole interaction with a given app dialog (opening, filling, closing) 
- A group of network requests to refresh/sync app data.

## Major Collections

Major collections may be manually performed by calling 
[GC.Collect()](xref:System.GC.Collect)
or `GC.Collect(GC.MaxGeneration)`. 

They should be performed rarely, and may have a pause time of a second 
on an Android-style device when collecting a 512MB heap. 

Major collections should only be manually invoked, if ever: 

- At the end of lengthy duty cycles and when a long pause won't 
    present a problem to the user. 

- Within an overridden
    [Android.App.Activity.OnLowMemory()](xref:Android.App.Activity.OnLowMemory) 
    method. 

## Diagnostics

To track when global references are created and destroyed, you can set 
the [debug.mono.log](~/android/troubleshooting/index.md) 
system property to contain 
[*gref*](~/android/troubleshooting/index.md) 
and/or 
[*gc*](~/android/troubleshooting/index.md). 

## Configuration

The Xamarin.Android garbage collector can be configured by setting the 
`MONO_GC_PARAMS` environment variable. Environment variables may be set
with a Build action of
[AndroidEnvironment](~/android/deploy-test/environment.md).

The `MONO_GC_PARAMS` environment variable is a comma-separated list of 
the following parameters: 

- `nursery-size` = *size* : Sets the size of the nursery. The size is 
    specified in bytes and must be a power of two. The suffixes `k` , 
    `m` and `g` can be used to specify kilo-, mega- and gigabytes, 
    respectively. The nursery is the first generation (of two). A 
    larger nursery will usually speed up the program but will obviously 
    use more memory. The default nursery size 512 kb. 

- `soft-heap-limit` = *size* : The target maximum managed memory 
    consumption for the app. When memory use is below the specified 
    value, the GC is optimized for execution time (fewer collections). 
    Above this limit, the GC is optimized for memory usage (more 
    collections). 

- `evacuation-threshold` = *threshold* : Sets the evacuation 
    threshold in percent. The value must be an integer in the range 0 
    to 100. The default is 66. If the sweep phase of the collection 
    finds that the occupancy of a specific heap block type is less than 
    this percentage, it will do a copying collection for that block 
    type in the next major collection, thereby restoring occupancy to 
    close to 100 percent. A value of 0 turns evacuation off. 

- `bridge-implementation` = *bridge implementation* : This will set 
    the GC Bridge option to help address GC performance issues. There 
    are three possible values: *old* , *new* , *tarjan*.

- `bridge-require-precise-merge`: The Tarjan bridge contains an
    optimization which may, on rare occasions, cause an object to
    be collected one GC after it first becomes garbage. Including
    this option disables that optimization, making GCs more
    predictable but potentially slower.

For example, to configure the GC to have a heap size limit of 128MB, 
add a new file to your Project with a **Build action** of 
`AndroidEnvironment` with the contents: 

```shell
MONO_GC_PARAMS=soft-heap-limit=128m
```
