---
title: "Xamarin.Forms DependencyService Registration and Resolution"
description: "This article explains how to register platform implementations with the Xamarin.Forms DependencyService class to invoke native platform functionality."
ms.service: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/05/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms DependencyService Registration and Resolution

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/dependencyservice/)

When using the Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) to invoke native platform functionality, platform implementations must be registered with the `DependencyService`, and then resolved from shared code to invoke them.

## Register platform implementations

Platform implementations must be registered with the [`DependencyService`](xref:Xamarin.Forms.DependencyService) so that Xamarin.Forms can locate them at runtime.

Registration can be performed with the [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute), or with the [`Register`](xref:Xamarin.Forms.DependencyService.Register*) and `RegisterSingleton` methods.

> [!IMPORTANT]
> Release builds of UWP projects that use .NET native compilation should register platform implementations with the [`Register`](xref:Xamarin.Forms.DependencyService.Register*) methods.

### Registration by attribute

The [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) can be used to register a platform implementation with the [`DependencyService`](xref:Xamarin.Forms.DependencyService). The attribute indicates that the specified type provides a concrete implementation of the interface.

The following example uses the [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) to register the iOS implementation of the `IDeviceOrientationService` interface:

```csharp
using Xamarin.Forms;

[assembly: Dependency(typeof(DeviceOrientationService))]
namespace DependencyServiceDemos.iOS
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            ...
        }
    }
}
```

In this example, the [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) registers the `DeviceOrientationService` with the [`DependencyService`](xref:Xamarin.Forms.DependencyService). This results in the concrete type being registered against the interface it implements.

Similarly, the implementations of the `IDeviceOrientationService` interface on other platforms should be registered with the [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute).

> [!NOTE]
> Registration with the [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) is performed at the namespace level.

### Registration by method

The [`DependencyService.Register`](xref:Xamarin.Forms.DependencyService.Register*) methods, and the `RegisterSingleton` method, can be used to register a platform implementation with the [`DependencyService`](xref:Xamarin.Forms.DependencyService).

The following example uses the [`Register`](xref:Xamarin.Forms.DependencyService.Register*) method to register the iOS implementation of the `IDeviceOrientationService` interface:

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new App());
        DependencyService.Register<IDeviceOrientationService, DeviceOrientationService>();
        return base.FinishedLaunching(app, options);
    }
}
```

In this example, the [`Register`](xref:Xamarin.Forms.DependencyService.Register*) method registers the concrete type, `DeviceOrientationService`, against the `IDeviceOrientationService` interface. Alternatively, an overload of the [`Register`](xref:Xamarin.Forms.DependencyService.Register*) method can be used to register a platform implementation with the [`DependencyService`](xref:Xamarin.Forms.DependencyService):

```csharp
DependencyService.Register<DeviceOrientationService>();
```

In this example, the [`Register`](xref:Xamarin.Forms.DependencyService.Register*) method registers the `DeviceOrientationService` with the [`DependencyService`](xref:Xamarin.Forms.DependencyService). This results in the concrete type being registered against the interface it implements.

Alternatively, an existing object instance can be registered as a singleton with the `RegisterSingleton` method:

```csharp
var service = new DeviceOrientationService();
DependencyService.RegisterSingleton<IDeviceOrientationService>(service);
```

In this example, the `RegisterSingleton` method registers the `DeviceOrientationService` object instance against the `IDeviceOrientationService` interface, as a singleton.

Similarly, the implementations of the `IDeviceOrientationService` interface on other platforms can be registered with the [`Register`](xref:Xamarin.Forms.DependencyService.Register*) methods, or the `RegisterSingleton` method.

> [!IMPORTANT]
> Registration with the [`Register`](xref:Xamarin.Forms.DependencyService.Register*) and `RegisterSingleton` methods must be performed in platform projects, before the functionality provided by the platform implementation is invoked from shared code.

## Resolve the platform implementations

Platform implementations must be resolved before being invoked. This is typically performed in shared code using the [`DependencyService.Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) method. However, it can also be accomplished with the [`DependencyService.Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) method.

By default, the [`DependencyService`](xref:Xamarin.Forms.DependencyService) will only resolve platform implementations that have parameterless constructors. However, a dependency resolution method can be injected into Xamarin.Forms that uses a dependency injection container or factory methods to resolve platform implementations. This approach can be used to resolve platform implementations that have constructors with parameters. For more information, see [Dependency resolution in Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md).

> [!IMPORTANT]
> Invoking a platform implementation that hasn't been registered with the [`DependencyService`](xref:Xamarin.Forms.DependencyService) will result in a `NullReferenceException` being thrown.

### Resolve using the Get&lt;T&gt; method

The [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) method retrieves the platform implementation of interface `T` at runtime, and either:

- Creates an instance of it as a singleton.
- Returns an existing instance as a singleton, that was registered with the `DependencyService` by the `RegisterSingleton` method.

In both cases, the instance will live for the lifetime of the application, and any subsequent calls to resolve the same platform implementation will retrieve the same instance.

The following code shows an example of calling the [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) method to resolve the `IDeviceOrientationService` interface, and then invoking its `GetOrientation` method:

```csharp
IDeviceOrientationService service = DependencyService.Get<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

Alternatively, this code can be condensed into a single line:

```csharp
DeviceOrientation orientation = DependencyService.Get<IDeviceOrientationService>().GetOrientation();
```

> [!NOTE]
> The [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) method returns an instance of the platform implementation of interface `T` as a singleton, by default. However, this behavior can be changed. For more information, see [Manage the lifetime of resolved objects](#manage-the-lifetime-of-resolved-objects).

### Resolve using the Resolve&lt;T&gt; method

The [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) method retrieves the platform implementation of interface `T` at runtime, using a dependency resolution method that's been injected into Xamarin.Forms with the [`DependencyResolver`](xref:Xamarin.Forms.Internals.DependencyResolver) class. If a dependency resolution method hasn't been injected into Xamarin.Forms, the `Resolve<T>` method will fallback to calling the [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) method to retrieve the platform implementation. For more information about injecting a dependency resolution method into Xamarin.Forms, see [Dependency resolution in Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md).

The following code shows an example of calling the [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) method to resolve the `IDeviceOrientationService` interface, and then invoking its `GetOrientation` method:

```csharp
IDeviceOrientationService service = DependencyService.Resolve<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

Alternatively, this code can be condensed into a single line:

```csharp
DeviceOrientation orientation = DependencyService.Resolve<IDeviceOrientationService>().GetOrientation();
```

> [!NOTE]
> When the [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) method falls back to calling the [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) method, it returns an instance of the platform implementation of interface `T` as a singleton, by default. However, this behavior can be changed. For more information, see [Manage the lifetime of resolved objects](#manage-the-lifetime-of-resolved-objects).

## Manage the lifetime of resolved objects

The default behavior of the [`DependencyService`](xref:Xamarin.Forms.DependencyService) class is to resolve platform implementations as singletons. Therefore, platform implementations will live for the lifetime of an application.

This behavior is specified with the [`DependencyFetchTarget`](xref:Xamarin.Forms.DependencyFetchTarget) optional argument on the [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) and [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) methods. The [`DependencyFetchTarget`](xref:Xamarin.Forms.DependencyFetchTarget) enumeration defines two members:

- `GlobalInstance`, which returns the platform implementation as a singleton.
- `NewInstance`, which returns a new instance of the platform implementation. The application is then responsible for managing the lifetime of the platform implementation instance.

The [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) and [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) methods both set their optional arguments to [`DependencyFetchTarget.GlobalInstance`](xref:Xamarin.Forms.DependencyFetchTarget), and so platform implementations are always resolved as singletons. This behavior can be changed, so that new instances of platform implementations are created, by specifying [`DependencyFetchTarget.NewInstance`](xref:Xamarin.Forms.DependencyFetchTarget) as arguments to the `Get<T>` and `Resolve<T>` methods:

```csharp
ITextToSpeechService service = DependencyService.Get<ITextToSpeechService>(DependencyFetchTarget.NewInstance);
```

In this example, the [`DependencyService`](xref:Xamarin.Forms.DependencyService) creates a new instance of the platform implementation for the `ITextToSpeechService` interface. Any subsequent calls to resolve the `ITextToSpeechService` will also create new instances.

The consequence of always creating a new instance of a platform implementation is that the application becomes responsible for managing the instances' lifetime. This means that if you subscribe to an event defined in a platform implementation, you should unsubscribe from the event when the platform implementation is no longer required. In addition, it means that it may be necessary for platform implementations to implement `IDisposable`, and cleanup their resources in `Dispose` methods. The sample application demonstrates this scenario in its `TextToSpeechService` platform implementations.

When an application finishes using a platform implementation that implements `IDisposable`, it should call the object's `Dispose` implementation. One way of accomplishing this is with a `using` statement:

```csharp
ITextToSpeechService service = DependencyService.Get<ITextToSpeechService>(DependencyFetchTarget.NewInstance);
using (service as IDisposable)
{
    await service.SpeakAsync("Hello world");
}
```

In this example, after the `SpeakAsync` method is invoked, the `using` statement automatically disposes of the platform implementation object. This results in the object's `Dispose` method being invoked, which performs the required cleanup.

For more information about calling an object's `Dispose` method, see [Using objects that implement IDisposable](/dotnet/standard/garbage-collection/using-objects).

## Related links

- [DependencyService Demos (sample)](/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [Dependency resolution in Xamarin.Forms](~/xamarin-forms/internals/dependency-resolution.md)