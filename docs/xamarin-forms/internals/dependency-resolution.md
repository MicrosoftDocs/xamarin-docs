---
title: "Dependency resolution in Xamarin.Forms"
description: "This article explains how to inject a dependency resolution method into Xamarin.Forms so that an application's dependency injection container has control over the creation and lifetime of custom renderers, effects, and DependencyService implementations."
ms.service: xamarin
ms.assetid: 491B87DC-14CB-4ADC-AC6C-40A7627B2524
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/27/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Dependency resolution in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo)

_This article explains how to inject a dependency resolution method into Xamarin.Forms so that an application's dependency injection container has control over the creation and lifetime of custom renderers, effects, and DependencyService implementations. The code examples in this article are taken from the [Dependency Resolution using Containers](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo) sample._

In the context of a Xamarin.Forms application that uses the Model-View-ViewModel (MVVM) pattern, a dependency injection container can be used for registering and resolving view models, and for registering services and injecting them into view models. During view model creation, the container injects any dependencies that are required. If those dependencies have not been created, the container creates and resolves the dependencies first. For more information about dependency injection, including examples of injecting dependencies into view models, see [Dependency Injection](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md).

Control over the creation and lifetime of types in platform projects is traditionally performed by Xamarin.Forms, which uses the `Activator.CreateInstance` method to create instances of custom renderers, effects, and [`DependencyService`](xref:Xamarin.Forms.DependencyService) implementations. Unfortunately, this limits developer control over the creation and lifetime of these types, and the ability to inject dependencies into them. This behavior can be changed by injecting a dependency resolution method into Xamarin.Forms that controls how types will be created – either by the application's dependency injection container, or by Xamarin.Forms. However, note that there is no requirement to inject a dependency resolution method into Xamarin.Forms. Xamarin.Forms will continue to create and manage the lifetime of types in platform projects if a dependency resolution method isn't injected.

> [!NOTE]
> While this article focuses on injecting a dependency resolution method into Xamarin.Forms that resolves registered types using a dependency injection container, it's also possible to inject a dependency resolution method that uses factory methods to resolve registered types. For more information, see the [Dependency Resolution using Factory Methods](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-factoriesdemo) sample.

## Injecting a dependency resolution method

The [`DependencyResolver`](xref:Xamarin.Forms.Internals.DependencyResolver) class provides the ability to inject a dependency resolution method into Xamarin.Forms, using the [`ResolveUsing`](xref:Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) method. Then, when Xamarin.Forms needs an instance of a particular type, the dependency resolution method is given the opportunity to provide the instance. If the dependency resolution method returns `null` for a requested type, Xamarin.Forms falls back to attempting to create the type instance itself using the `Activator.CreateInstance` method.

The following example shows how to set the dependency resolution method with the [`ResolveUsing`](xref:Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) method:

```csharp
using Autofac;
using Xamarin.Forms.Internals;
...

public partial class App : Application
{
    // IContainer and ContainerBuilder are provided by Autofac
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();

    public App()
    {
        ...
        DependencyResolver.ResolveUsing(type => container.IsRegistered(type) ? container.Resolve(type) : null);
        ...
    }
    ...
}
```

In this example, the dependency resolution method is set to a lambda expression that uses the Autofac dependency injection container to resolve any types that have been registered with the container. Otherwise, `null` will be returned, which will result in Xamarin.Forms attempting to resolve the type.

> [!NOTE]
> The API used by a dependency injection container is specific to the container. The code examples in this article use Autofac as a dependency injection container, which provides the `IContainer` and `ContainerBuilder` types. Alternative dependency injection containers could equally be used, but would use different APIs than are presented here.

Note that there is no requirement to set the dependency resolution method during application startup. It can be set at any time. The only constraint is that Xamarin.Forms needs to know about the dependency resolution method by the time that the application attempts to consume types stored in the dependency injection container. Therefore, if there are services in the dependency injection container that the application will require during startup, the dependency resolution method will have to be set early in the application's lifecycle. Similarly, if the dependency injection container manages the creation and lifetime of a particular [`Effect`](xref:Xamarin.Forms.Effect), Xamarin.Forms will need to know about the dependency resolution method before it attempts to create a view that uses that `Effect`.

> [!WARNING]
> Registering and resolving types with a dependency injection container has a performance cost because of the container's use of reflection for creating each type, especially if dependencies are being reconstructed for each page navigation in the application. If there are many or deep dependencies, the cost of creation can increase significantly.

## Registering types

Types must be registered with the dependency injection container before it can resolve them via the dependency resolution method. The following code example shows the registration methods that the sample application exposes in the `App` class, for the Autofac container:

```csharp
using Autofac;
using Autofac.Core;
...

public partial class App : Application
{
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();
    ...

    public static void RegisterType<T>() where T : class
    {
        builder.RegisterType<T>();
    }

    public static void RegisterType<TInterface, T>() where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>().As<TInterface>();
    }

    public static void RegisterTypeWithParameters<T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where T : class
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        });
    }

    public static void RegisterTypeWithParameters<TInterface, T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        }).As<TInterface>();
    }

    public static void BuildContainer()
    {
        container = builder.Build();
    }
    ...
}
```

When an application uses a dependency resolution method to resolve types from a container, type registrations are typically performed from platform projects. This enables platform projects to register types for custom renderers, effects, and [`DependencyService`](xref:Xamarin.Forms.DependencyService) implementations.

Following type registration from a platform project, the `IContainer` object must be built, which is accomplished by calling the `BuildContainer` method. This method invokes Autofac's `Build` method on the `ContainerBuilder` instance, which builds a new dependency injection container that contains the registrations that have been made.

In the sections that follow, a `Logger` class that implements the `ILogger` interface is injected into class constructors. The `Logger` class implements simple logging functionality using the `Debug.WriteLine` method, and is used to demonstrate how services can be injected into custom renderers, effects, and [`DependencyService`](xref:Xamarin.Forms.DependencyService) implementations.

### Registering custom renderers

The sample application includes a page that plays web videos, whose XAML source is shown in the following example:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             ...>
    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />
</ContentPage>
```

The `VideoPlayer` view is implemented on each platform by a `VideoPlayerRenderer` class, that provides the functionality for playing the video. For more information about these custom renderer classes, see [Implementing a video player](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md).

On iOS and the Universal Windows Platform (UWP), the `VideoPlayerRenderer` classes have the following constructor, which requires an `ILogger` argument:

```csharp
public VideoPlayerRenderer(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

On all the platforms, type registration with the dependency injection container is performed by the `RegisterTypes` method, which is invoked prior to the platform loading the application with the `LoadApplication(new App())` method. The following example shows the `RegisterTypes` method on the iOS platform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<FormsVideoLibrary.iOS.VideoPlayerRenderer>();
    App.BuildContainer();
}
```

In this example, the `Logger` concrete type is registered via a mapping against its interface type, and the `VideoPlayerRenderer` type is registered directly without an interface mapping. When the user navigates to the page containing the `VideoPlayer` view, the dependency resolution method will be invoked to resolve the `VideoPlayerRenderer` type from the dependency injection container, which will also resolve and inject the `Logger` type into the `VideoPlayerRenderer` constructor.

The `VideoPlayerRenderer` constructor on the Android platform is slightly more complicated as it requires a `Context` argument in addition to the `ILogger` argument:

```csharp
public VideoPlayerRenderer(Context context, ILogger logger) : base(context)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

The following example shows the `RegisterTypes` method on the Android platform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<FormsVideoLibrary.Droid.VideoPlayerRenderer>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

In this example, the `App.RegisterTypeWithParameters` method registers the `VideoPlayerRenderer` with the dependency injection container. The registration method ensures that the `MainActivity` instance will be injected as the `Context` argument, and that the `Logger` type will be injected as the `ILogger` argument.

### Registering effects

The sample application includes a page that uses a touch tracking effect to drag [`BoxView`](xref:Xamarin.Forms.BoxView) instances around the page. The [`Effect`](xref:Xamarin.Forms.Effect) is added to the `BoxView` using the following code:

```csharp
var boxView = new BoxView { ... };
var touchEffect = new TouchEffect();
boxView.Effects.Add(touchEffect);
```

The `TouchEffect` class is a [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) that's implemented on each platform by a `TouchEffect` class that's a `PlatformEffect`. The platform `TouchEffect` class provides the functionality for dragging the `BoxView` around the page. For more information about these effect classes, see [Invoking events from effects](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md).

On all the platforms, the `TouchEffect` class has the following constructor, which requires an `ILogger` argument:

```csharp
public TouchEffect(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

On all the platforms, type registration with the dependency injection container is performed by the `RegisterTypes` method, which is invoked prior to the platform loading the application with the `LoadApplication(new App())` method. The following example shows the `RegisterTypes` method on the Android platform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<TouchTracking.Droid.TouchEffect>();
    App.BuildContainer();
}
```

In this example, the `Logger` concrete type is registered via a mapping against its interface type, and the `TouchEffect` type is registered directly without an interface mapping. When the user navigates to the page containing a [`BoxView`](xref:Xamarin.Forms.BoxView) instance that has the `TouchEffect` attached to it, the dependency resolution method will be invoked to resolve the platform `TouchEffect` type from the dependency injection container, which will also resolve and inject the `Logger` type into the `TouchEffect` constructor.

### Registering DependencyService implementations

The sample application includes a page that uses [`DependencyService`](xref:Xamarin.Forms.DependencyService) implementations on each platform to allow the user to pick a photo from the device's picture library. The `IPhotoPicker` interface defines the functionality that is implemented by the `DependencyService` implementations, and is shown in the following example:

```csharp
public interface IPhotoPicker
{
    Task<Stream> GetImageStreamAsync();
}
```

In each platform project, the `PhotoPicker` class implements the `IPhotoPicker` interface using platform APIs. For more information about these dependency services, see [Picking a photo from the picture library](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

On iOS and UWP, the `PhotoPicker` classes have the following constructor, which requires an `ILogger` argument:

```csharp
public PhotoPicker(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

On all the platforms, type registration with the dependency injection container is performed by the `RegisterTypes` method, which is invoked prior to the platform loading the application with the `LoadApplication(new App())` method. The following example shows the `RegisterTypes` method on UWP:

```csharp
void RegisterTypes()
{
    DIContainerDemo.App.RegisterType<ILogger, Logger>();
    DIContainerDemo.App.RegisterType<IPhotoPicker, Services.UWP.PhotoPicker>();
    DIContainerDemo.App.BuildContainer();
}
```

In this example, the `Logger` concrete type is registered via a mapping against its interface type, and the `PhotoPicker` type is also registered via a interface mapping.

The `PhotoPicker` constructor on the Android platform is slightly more complicated as it requires a `Context` argument in addition to the `ILogger` argument:

```csharp
public PhotoPicker(Context context, ILogger logger)
{
    _context = context ?? throw new ArgumentNullException(nameof(context));
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

The following example shows the `RegisterTypes` method on the Android platform:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<IPhotoPicker, Services.Droid.PhotoPicker>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

In this example, the `App.RegisterTypeWithParameters` method registers the `PhotoPicker` with the dependency injection container. The registration method ensures that the `MainActivity` instance will be injected as the `Context` argument, and that the `Logger` type will be injected as the `ILogger` argument.

When the user navigates to the photo picking page and chooses to select a photo, the `OnSelectPhotoButtonClicked` handler is executed:

```csharp
async void OnSelectPhotoButtonClicked(object sender, EventArgs e)
{
    ...
    var photoPickerService = DependencyService.Resolve<IPhotoPicker>();
    var stream = await photoPickerService.GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }
    ...
}
```

When the [`DependencyService.Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) method is invoked, the dependency resolution method will be invoked to resolve the `PhotoPicker` type from the dependency injection container, which will also resolve and inject the `Logger` type into the `PhotoPicker` constructor.

> [!NOTE]
> The [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) method must be used when resolving a type from the application's dependency injection container via the [`DependencyService`](xref:Xamarin.Forms.DependencyService).

## Related links

- [Dependency resolution using containers (sample)](/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo)
- [Dependency injection](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)
- [Implementing a video player](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)
- [Invoking events from effects](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
- [Picking a photo from the picture library](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)