---
title: "Improve Xamarin.Forms App Performance"
description: "There are many techniques for increasing the performance of Xamarin.Forms applications. Collectively these techniques can greatly reduce the amount of work being performed by a CPU, and the amount of memory consumed by an application."
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/27/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Improve Xamarin.Forms App Performance

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**Evolve 2016: Optimizing App Performance with Xamarin.Forms**

Poor application performance presents itself in many ways. It can make an application seem unresponsive, can cause slow scrolling, and can reduce device battery life. However, optimizing performance involves more than just implementing efficient code. The user's experience of application performance must also be considered. For example, ensuring that operations execute without blocking the user from performing other activities can help to improve the user's experience.

There are many techniques for increasing the performance, and perceived performance, of Xamarin.Forms applications. Collectively these techniques can greatly reduce the amount of work being performed by a CPU, and the amount of memory consumed by an application.

> [!NOTE]
> Before reading this article you should first read [Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md), which discusses non-platform specific techniques to improve the memory usage and performance of applications built using the Xamarin platform.

## Enable the XAML compiler

XAML can be optionally compiled directly into intermediate language (IL) with the XAML compiler (XAMLC). XAMLC offers a number of benefits:

- It performs compile-time checking of XAML, notifying the user of any errors.
- It removes some of the load and instantiation time for XAML elements.
- It helps to reduce the file size of the final assembly by no longer including .xaml files.

XAMLC is enabled by default in new Xamarin.Forms solutions. However, it may need to be enabled in older solutions. For more information, see [Compiling XAML](~/xamarin-forms/xaml/xamlc.md).

## Use compiled bindings

Compiled bindings improve data binding performance in Xamarin.Forms applications by resolving binding expressions at compile time, rather than at runtime with reflection. Compiling a binding expression generates compiled code that typically resolves a binding 8-20 times quicker than using a classic binding. For more information, see [Compiled Bindings](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

## Reduce unnecessary bindings

Don't use bindings for content that can easily be set statically. There is no advantage in binding data that doesn't need to be bound, because bindings aren't cost efficient. For example, setting `Button.Text = "Accept"` has less overhead than binding [`Button.Text`](xref:Xamarin.Forms.Button.Text) to a viewmodel `string` property with value "Accept".

## Use fast renderers

Fast renderers reduce the inflation and rendering costs of Xamarin.Forms controls on Android by flattening the resulting native control hierarchy. This further improves performance by creating fewer objects, which in turns results in a less complex visual tree, and less memory use.

From Xamarin.Forms 4.0 onwards, all applications targeting `FormsAppCompatActivity` use fast renderers by default. For more information, see [Fast Renderers](~/xamarin-forms/internals/fast-renderers.md).

## Enable startup tracing on Android

Ahead of Time (AOT) compilation on Android minimizes Just in Time (JIT) application startup overhead and memory usage, at the cost of creating a much larger APK. An alternative is to use startup tracing, which provides a trade-off between Android APK size and startup time, when compared to conventional AOT compilation.

Instead of compiling as much of the application as possible to unmanaged code, startup tracing compiles only the set of managed methods that represent the most expensive parts of application startup in a blank Xamarin.Forms application. This approach results in a reduced APK size, when compared to conventional AOT compilation, while still providing similar startup improvements.

## Enable layout compression

Layout compression removes specified layouts from the visual tree, in an attempt to improve page rendering performance. The performance benefit that this delivers varies depending on the complexity of a page, the version of the operating system being used, and the device on which the application is running. However, the biggest performance gains will be seen on older devices. For more information, see [Layout Compression](~/xamarin-forms/user-interface/layouts/layout-compression.md).

## Choose the correct layout

A layout that's capable of displaying multiple children, but that only has a single child, is wasteful. For example, the following code example shows a [`StackLayout`](xref:Xamarin.Forms.StackLayout) with a single child:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <StackLayout>
        <Image Source="waterfront.jpg" />
    </StackLayout>
</ContentPage>
```

This is wasteful and the [`StackLayout`](xref:Xamarin.Forms.StackLayout) element should be removed, as shown in the following code example:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <Image Source="waterfront.jpg" />
</ContentPage>
```

In addition, don't attempt to reproduce the appearance of a specific layout by using combinations of other layouts, as this results in unnecessary layout calculations being performed. For example, don't attempt to reproduce a [`Grid`](xref:Xamarin.Forms.Grid) layout by using a combination of [`StackLayout`](xref:Xamarin.Forms.StackLayout) instances. The following code example shows an example of this bad practice:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Name:" />
            <Entry Placeholder="Enter your name" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Age:" />
            <Entry Placeholder="Enter your age" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Occupation:" />
            <Entry Placeholder="Enter your occupation" />
        </StackLayout>
        <StackLayout Orientation="Horizontal">
            <Label Text="Address:" />
            <Entry Placeholder="Enter your address" />
        </StackLayout>
    </StackLayout>
</ContentPage>
```

This is wasteful because unnecessary layout calculations are performed. Instead, the desired layout can be better achieved using a [`Grid`](xref:Xamarin.Forms.Grid), as shown in the following code example:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="100" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
            <RowDefinition Height="30" />
        </Grid.RowDefinitions>
        <Label Text="Name:" />
        <Entry Grid.Column="1" Placeholder="Enter your name" />
        <Label Grid.Row="1" Text="Age:" />
        <Entry Grid.Row="1" Grid.Column="1" Placeholder="Enter your age" />
        <Label Grid.Row="2" Text="Occupation:" />
        <Entry Grid.Row="2" Grid.Column="1" Placeholder="Enter your occupation" />
        <Label Grid.Row="3" Text="Address:" />
        <Entry Grid.Row="3" Grid.Column="1" Placeholder="Enter your address" />
    </Grid>
</ContentPage>
```

## Optimize layout performance

To obtain the best possible layout performance, follow these guidelines:

- Reduce the depth of layout hierarchies by specifying [`Margin`](xref:Xamarin.Forms.View.Margin) property values, allowing the creation of layouts with fewer wrapping views. For more information, see [Margins and Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).
- When using a [`Grid`](xref:Xamarin.Forms.Grid), try to ensure that as few rows and columns as possible are set to [`Auto`](xref:Xamarin.Forms.GridLength.Auto) size. Each auto-sized row or column will cause the layout engine to perform additional layout calculations. Instead, use fixed size rows and columns if possible. Alternatively, set rows and columns to occupy a proportional amount of space with the [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) enumeration value, provided that the parent tree follows these layout guidelines.
- Don't set the [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) and [`HorizontalOptions`](xref:Xamarin.Forms.View.VerticalOptions) properties of a layout unless required. The default values of [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) and [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) allow for the best layout optimization. Changing these properties has a cost and consumes memory, even when setting them to the default values.
- Avoid using a [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) whenever possible. It will result in the CPU having to perform significantly more work.
- When using an [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout), avoid using the [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) property whenever possible.
- When using a [`StackLayout`](xref:Xamarin.Forms.StackLayout), ensure that only one child is set to [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands). This property ensures that the specified child will occupy the largest space that the `StackLayout` can give to it, and it is wasteful to perform these calculations more than once.
- Avoid calling any of the methods of the [`Layout`](xref:Xamarin.Forms.Layout) class, as they result in expensive layout calculations being performed. Instead, it's likely that the desired layout behavior can be obtained by setting the [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) and [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) properties. Alternatively, subclass the [`Layout<View>`](xref:Xamarin.Forms.Layout`1) class to achieve the desired layout behavior.
- Don't update any [`Label`](xref:Xamarin.Forms.Label) instances more frequently than required, as the change of size of the label can result in the entire screen layout being re-calculated.
- Don't set the [`Label.VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) property unless required.
- Set the [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) of any [`Label`](xref:Xamarin.Forms.Label) instances to [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap) whenever possible.

## Use asynchronous programming

The overall responsiveness of your application can be enhanced, and performance bottlenecks often avoided, by using asynchronous programming. In .NET, the [Task-based Asynchronous Pattern (TAP)](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap) is the recommended design pattern for asynchronous operations. However, incorrect use of the TAP can result in unperformant applications. Therefore, the following guidelines should be followed when using the TAP.

### Fundamentals

- Understand the task lifecycle, which is represented by the `TaskStatus` enumeration. For more information, see [The meaning of TaskStatus](https://devblogs.microsoft.com/pfxteam/the-meaning-of-taskstatus/) and [Task status](/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap#task-status).
- Use the `Task.WhenAll` method to asynchronously wait for multiple asynchronous operations to finish, rather than individually `await` a series of asynchronous operations. For more information, see [Task.WhenAll](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskwhenall).
- Use the `Task.WhenAny` method to asynchronously wait for one of multiple asynchronous operations to finish. For more information, see [Task.WhenAny](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskwhenall).
- Use the `Task.Delay` method to produce a `Task` object that finishes after the specified time. This is useful for scenarios such as polling for data, and delaying handling user input for a predetermined time. For more information, see [Task.Delay](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskdelay).
- Execute intensive synchronous CPU operations on the thread pool with the `Task.Run` method. This method is a shortcut for the `TaskFactory.StartNew` method, with the most optimal arguments set. For more information, see [Task.Run](/dotnet/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern#taskrun).
- Avoid trying to create asynchronous constructors. Instead, use lifecycle events or separate initialization logic to correctly `await` any initialization. For more information, see [Async Constructors](https://blog.stephencleary.com/2013/01/async-oop-2-constructors.html) on blog.stephencleary.com.
- Use the lazy task pattern to avoid waiting for asynchronous operations to complete during application startup. For more information, see [AsyncLazy](https://devblogs.microsoft.com/pfxteam/asynclazyt/).
- Create a task wrapper for existing asynchronous operations, that don't use the TAP, by creating `TaskCompletionSource<T>` objects. These objects gain the benefits of `Task` programmability, and enable you to control the lifetime and completion of the associated `Task`. For more information, see [The Nature of TaskCompletionSource](https://devblogs.microsoft.com/pfxteam/the-nature-of-taskcompletionsourcetresult/).
 
- Return a `Task` object, instead of returning an awaited `Task` object, when there's no need to process the result of an asynchronous operation. This is more performant due to less context switching being performed.
- Use the Task Parallel Library (TPL) Dataflow library in scenarios such as processing data as it becomes available, or when you have multiple operations that must communicate with each other asynchronously. For more information, see [Dataflow (Task Parallel Library)](/dotnet/standard/parallel-programming/dataflow-task-parallel-library).

### UI

- Call an asynchronous version of an API, if it's available. This will keep the UI thread unblocked, which will help to improve the user's experience with the application.
- Update UI elements with data from asynchronous operations on the UI thread, to avoid exceptions being thrown. However, updates to the `ListView.ItemsSource` property will automatically be marshaled to the UI thread. For information about determining if code is running on the UI thread, see [Xamarin.Essentials: MainThread](~/essentials/main-thread.md?content=xamarin/xamarin-forms).

    > [!IMPORTANT]
    > Any control properties that are updated via data binding will be automatically marshaled to the UI thread.

### Error handling

- Learn about asynchronous exception handling. Unhandled exceptions that are thrown by code that's running asynchronously are propagated back to the calling thread, except in certain scenarios. For more information, see [Exception handling (Task Parallel Library)](/dotnet/standard/parallel-programming/exception-handling-task-parallel-library).
- Avoid creating `async void` methods, and instead create `async Task` methods. These enable easier error-handling, composability, and testability. The exception to this guideline is asynchronous event handlers, which must return `void`. For more information, see [Avoid Async Void](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#avoid-async-void).
- Don't mix blocking and asynchronous code by calling the `Task.Wait`, `Task.Result`, or `GetAwaiter().GetResult` methods, as they can result in deadlock occurring. However, if this guideline must be violated, the preferred approach is to call the `GetAwaiter().GetResult` method because it preserves the task exceptions. For more information, see [Async All the Way](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#async-all-the-way) and [Task Exception Handling in .NET 4.5](https://devblogs.microsoft.com/pfxteam/task-exception-handling-in-net-4-5/).
- Use the `ConfigureAwait` method whenever possible, to create context-free code. Context-free code has better performance for mobile applications and is a useful technique for avoiding deadlock when working with a partially asynchronous codebase. For more information, see [Configure Context](/archive/msdn-magazine/2013/march/async-await-best-practices-in-asynchronous-programming#configure-context).
- Use *continuation tasks* for functionality such as handling exceptions thrown by the previous asynchronous operation, and canceling a continuation either before it starts or while it is running. For more information, see [Chaining Tasks by Using Continuous Tasks](/dotnet/standard/parallel-programming/chaining-tasks-by-using-continuation-tasks).
- Use an asynchronous `ICommand` implementation when asynchronous operations are invoked from the `ICommand`. This ensures that any exceptions in the asynchronous command logic can be handled. For more information, see [Async Programming: Patterns for Asynchronous MVVM Applications: Commands](/archive/msdn-magazine/2014/april/async-programming-patterns-for-asynchronous-mvvm-applications-commands).

## Choose a dependency injection container carefully

Dependency injection containers introduce additional performance constraints into mobile applications. Registering and resolving types with a container has a performance cost because of the container's use of reflection for creating each type, especially if dependencies are being reconstructed for each page navigation in the app. If there are many or deep dependencies, the cost of creation can increase significantly. In addition, type registration, which usually occurs during application startup, can have a noticeable impact on startup time, dependent upon the container being used.

As an alternative, dependency injection can be made more performant by implementing it manually using factories.

## Create Shell applications

Xamarin.Forms Shell applications provide an opinionated navigation experience based on flyouts and tabs. If your application user experience can be implemented with Shell, it is beneficial to do so. Shell applications help to avoid a poor startup experience, because pages are created on demand in response to navigation rather than at application startup, which occurs with applications that use a [`TabbedPage'](xref:Xamarin.Forms.TabbedPage). For more information, see [Xamarin.Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md).

## Use CollectionView instead of ListView

[`CollectionView`](xref:Xamarin.Forms.CollectionView) is a view for presenting lists of data using different layout specifications. It provides a more flexible, and performant alternative to [`ListView`](xref:Xamarin.Forms.ListView). For more information, see [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md).

## Optimize ListView performance

When using [`ListView`](xref:Xamarin.Forms.ListView), there are a number of user experiences that should be optimized:

- **Initialization** – the time interval starting when the control is created, and ending when items are shown on screen.
- **Scrolling** – the ability to scroll through the list and ensure that the UI doesn't lag behind touch gestures.
- **Interaction** for adding, deleting, and selecting items.

The [`ListView`](xref:Xamarin.Forms.ListView) control requires an application to supply data and cell templates. How this is achieved will have a large impact on the performance of the control. For more information, see [ListView Performance](~/xamarin-forms/user-interface/listview/performance.md).

## Optimize image resources

Displaying image resources can greatly increase an application's memory footprint. Therefore, they should only be created when required and should be released as soon as the application no longer requires them. For example, if an application is displaying an image by reading its data from a stream, ensure that stream is created only when it's required, and ensure that the stream is released when it's no longer required. This can be achieved by creating the stream when the page is created, or when the [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) event fires, and then disposing of the stream when the [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) event fires.

When downloading an image for display with the [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) method, cache the downloaded image by ensuring that the [`UriImageSource.CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) property is set to `true`. For more information, see [Working with Images](~/xamarin-forms/user-interface/images.md).

For more information, see [Optimize Image Resources](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages).

## Reduce the visual tree size

Reducing the number of elements on a page will make the page render faster. There are two main techniques for achieving this. The first is to hide elements that aren't visible. The [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) property of each element determines whether the element should be part of the visual tree or not. Therefore, if an element isn't visible because it's hidden behind other elements, either remove the element or set its `IsVisible` property to `false`.

The second technique is to remove unnecessary elements. For example, the following code example shows a page layout containing multiple [`Label`](xref:Xamarin.Forms.Label) objects:

```xaml
<StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Hello" />
    </StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Welcome to the App!" />
    </StackLayout>
    <StackLayout Padding="20,20,0,0">
        <Label Text="Downloading Data..." />
    </StackLayout>
</StackLayout>
```

The same page layout can be maintained with a reduced element count, as shown in the following code example:

```xaml
<StackLayout Padding="20,35,20,20" Spacing="25">
  <Label Text="Hello" />
  <Label Text="Welcome to the App!" />
  <Label Text="Downloading Data..." />
</StackLayout>
```

## Reduce the application resource dictionary size

Any resources that are used throughout the application should be stored in the application's resource dictionary to avoid duplication. This will help to reduce the amount of XAML that has to be parsed throughout the application. The following code example shows the `HeadingLabelStyle` resource, which is used application wide, and so is defined in the application's resource dictionary:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Resources.App">
     <Application.Resources>
         <ResourceDictionary>
            <Style x:Key="HeadingLabelStyle" TargetType="Label">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
         </ResourceDictionary>
     </Application.Resources>
</Application>
```

However, XAML that's specific to a page shouldn't be included in the application's resource dictionary, as the resources will then be parsed at application startup instead of when required by a page. If a resource is used by a page that's not the startup page, it should be placed in the resource dictionary for that page, therefore helping to reduce the XAML that's parsed when the application starts. The following code example shows the `HeadingLabelStyle` resource, which is only on a single page, and so is defined in the page's resource dictionary:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Test.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Resources>
        <ResourceDictionary>
          <Style x:Key="HeadingLabelStyle" TargetType="Label">
              <Setter Property="HorizontalOptions" Value="Center" />
              <Setter Property="FontSize" Value="Large" />
              <Setter Property="TextColor" Value="Red" />
          </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    ...
</ContentPage>
```

For more information about application resources, see [XAML Styles](~/xamarin-forms/user-interface/styles/xaml/index.md).

## Use the custom renderer pattern

Most Xamarin.Forms renderer classes expose the `OnElementChanged` method, which is called when a Xamarin.Forms custom control is created to render the corresponding native control. Custom renderer classes, in each platform project, then override this method to instantiate and customize the native control. The `SetNativeControl` method is used to instantiate the native control, and this method will also assign the control reference to the `Control` property.

However, in some circumstances the `OnElementChanged` method can be called multiple times. Therefore, to prevent memory leaks, which can have a performance impact, care must be taken when instantiating a new native control. The approach to use when instantiating a new native control in a custom renderer is shown in the following code example:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null)
  {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null)
  {
    if (Control == null)
    {
      // Instantiate the native control with the SetNativeControl method
    }
    // Configure the control and subscribe to event handlers
  }
}
```

A new native control should only be instantiated once, when the `Control` property is `null`. In addition, the control should only be created, configured, and event handlers subscribed to when the custom renderer is attached to a new Xamarin.Forms element. Similarly, any event handlers that were subscribed to should only be unsubscribed from when the element the renderer is attached to changes. Adopting this approach will help to create an efficiently performing custom renderer that doesn't suffer from memory leaks.

> [!IMPORTANT]
> The `SetNativeControl` method should only be invoked if the `e.NewElement` property is not `null`, and the `Control` property is `null`.

For more information about custom renderers, see [Customizing Controls on Each Platform](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## Related links

- [Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Compiling XAML](~/xamarin-forms/xaml/xamlc.md)
- [Compiled Bindings](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)
- [Fast Renderers](~/xamarin-forms/internals/fast-renderers.md)
- [Layout Compression](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [Xamarin.Forms Shell](~/xamarin-forms/app-fundamentals/shell/index.md)
- [Xamarin.Forms CollectionView](~/xamarin-forms/user-interface/collectionview/index.md)
- [ListView Performance](~/xamarin-forms/user-interface/listview/performance.md)
- [Optimize Image Resources](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)
- [XAML Styles](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [Customizing Controls on Each Platform](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
