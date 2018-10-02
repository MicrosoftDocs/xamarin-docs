---
title: "Xamarin.Android vs. Desktop - Differences in the Mono Runtime"
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
---

# Limitations

Since applications on Android require generating Java proxy types during the build process, it is not possible to generate all code at runtime.

These are the Xamarin.Android limitations compared to desktop Mono:


## Limited Dynamic Language Support

 [Android callable wrappers](~/android/platform/java-integration/android-callable-wrappers.md) are needed any time the Android runtime needs to invoke managed code. Android callable wrappers are generated at compile time, based on static analysis of IL. The net result of this: you *cannot* use dynamic languages (IronPython, IronRuby, etc.) in any scenario where subclassing of Java types is required (including indirect subclassing), as there's no way of extracting these dynamic types at compile time to generate the necessary Android callable wrappers.


## Limited Java Generation Support

[Android Callable Wrappers](~/android/platform/java-integration/android-callable-wrappers.md) need to be generated in order for Java code to call managed code. *By default*, Android callable wrappers will only contain (certain) declared constructors and methods which override a virtual Java method (i.e. it has [`RegisterAttribute`](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) or implement a Java interface method (interface likewise has `Attribute`).
  
Prior to the 4.1 release, no additional methods could be declared. With the
4.1 release, [the `Export` and `ExportField` custom attributes can be used to declare Java methods and fields within the Android Callable Wrapper](~/android/platform/java-integration/working-with-jni.md).

### Missing constructors

Constructors remain tricky, unless [`ExportAttribute`](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute) is used. The algorithm for generating Android callable wrapper constructors is that a Java constructor will be emitted if:

1. There is a Java mapping for all the parameter types
2. The base class declares the same constructor &ndash; This is required because the Android callable wrapper *must* invoke the corresponding base class constructor; no default arguments can be used (as there's no easy way to determine what values should be used within Java).

For example, consider the following class:

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

While this looks perfectly logical, the resulting Android callable wrapper *in Release builds* will not contain a default constructor. Consequently, if you attempt to start this service (e.g. [`Context.StartService`](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)), it will fail:

```shell
E/AndroidRuntime(31766): FATAL EXCEPTION: main
E/AndroidRuntime(31766): java.lang.RuntimeException: Unable to instantiate service example.MyIntentService: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2347)
E/AndroidRuntime(31766):        at android.app.ActivityThread.access$1600(ActivityThread.java:130)
E/AndroidRuntime(31766):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1277)
E/AndroidRuntime(31766):        at android.os.Handler.dispatchMessage(Handler.java:99)
E/AndroidRuntime(31766):        at android.os.Looper.loop(Looper.java:137)
E/AndroidRuntime(31766):        at android.app.ActivityThread.main(ActivityThread.java:4745)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invokeNative(Native Method)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invoke(Method.java:511)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:786)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:553)
E/AndroidRuntime(31766):        at dalvik.system.NativeStart.main(Native Method)
E/AndroidRuntime(31766): Caused by: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at java.lang.Class.newInstanceImpl(Native Method)
E/AndroidRuntime(31766):        at java.lang.Class.newInstance(Class.java:1319)
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2344)
E/AndroidRuntime(31766):        ... 10 more
```

The workaround is to declare a default constructor, adorn it with the `ExportAttribute`,  and set the [`ExportAttribute.SuperStringArgument`](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

```csharp
[Service]
class MyIntentService : IntentService {
    [Export (SuperArgumentsString = "\"value\"")]
    public MyIntentService (): base("value")
    {
    }

    // ...
}
```


### Generic C# classes

Generic C# classes are only partially supported. The following limitations exist:


-   Generic types may not use `[Export]` or `[ExportField`]. Attempting
    to do so will generate an `XA4207` error.

    ```csharp
    public abstract class Parcelable<T> : Java.Lang.Object, IParcelable
    {
        // Invalid; generates XA4207
        [ExportField ("CREATOR")]
        public static IParcelableCreator CreateCreator ()
        {
            ...
    }
    ```

-   Generic methods may not use `[Export]` or `[ExportField]`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        
        // Invalid; generates XA4207
        [Export]
        public static void Method<T>(T value)
        {
            ...
        }
    }
    ```

-   `[ExportField]` may not be used on methods which return `void`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        // Invalid; generates XA4208
        [ExportField ("CREATOR")]
        public static void CreateSomething ()
        {
        }
    }
    ```

-   Instances of Generic types _must not_ be created from Java code.
    They can only safely be created from managed code:

    ```csharp
    [Activity (Label="Die!", MainLauncher=true)]
    public class BadGenericActivity<T> : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);
        }
    }
    ```


## Partial Java Generics Support

The Java generics binding support is limited. Particularly, members in
a generic instance class that is derived from another generic
(non-instantiated) class are left exposed as Java.Lang.Object. For
example, [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/)
method returns Java.Lang.Object. This is due to erased Java generics.
We have some classes that do not apply this limitation, but they are
manually adjusted.


## Related Links

- [Android Callable Wrappers](~/android/platform/java-integration/android-callable-wrappers.md)
- [Working with JNI](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
