---
title: "Xamarin.Forms Performance"
description: "There are many techniques for increasing the performance of Xamarin.Forms applications. Collectively these techniques can greatly reduce the amount of work being performed by a CPU, and the amount of memory consumed by an application. This article describes and discusses these techniques."
ms.prod: xamarin
ms.assetid: 0be84c56-6698-448d-be5a-b4205f1caa9f
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
---

# Xamarin.Forms Performance

_There are many techniques for increasing the performance of Xamarin.Forms applications. Collectively these techniques can greatly reduce the amount of work being performed by a CPU, and the amount of memory consumed by an application. This article describes and discusses these techniques._

> [!VIDEO https://youtube.com/embed/RZvdql3Ev0E]

**Evolve 2016: Optimizing App Performance with Xamarin.Forms**

## Overview

Poor application performance presents itself in many ways. It can make an application seem unresponsive, can cause slow scrolling, and can reduce battery life. However, optimizing performance involves more than just implementing efficient code. The user's experience of application performance must also be considered. For example, ensuring that operations execute without blocking the user from performing other activities can help to improve the user's experience.

There are a number of techniques for increasing the performance, and perceived performance, of a Xamarin.Forms application. They include:

- [Enable the XAML Compiler](#xamlc)
- [Choose the Correct Layout](#correctlayout)
- [Enable Layout Compression](#layoutcompression)
- [Use Fast Renderers](#fastrenderers)
- [Reduce Unnecessary Bindings](#databinding)
- [Optimize Layout Performance](#optimizelayout)
- [Optimize ListView Performance](#optimizelistview)
- [Optimize Image Resources](#optimizeimages)
- [Reduce the Visual Tree Size](#visualtree)
- [Reduce the Application Resource Dictionary Size](#resourcedictionary)
- [Use the Custom Renderer Pattern](#rendererpattern)

> [!NOTE]
>  Before reading this article you should first read [Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md), which discusses non-platform specific techniques to improve the memory usage and performance of applications built using the Xamarin platform.

<a name="xamlc" />

## Enable the XAML Compiler

XAML can be optionally compiled directly into intermediate language (IL) with the XAML compiler (XAMLC). XAMLC offers a number of a benefits:

- It performs compile-time checking of XAML, notifying the user of any errors.
- It removes some of the load and instantiation time for XAML elements.
- It helps to reduce the file size of the final assembly by no longer including .xaml files.

XAMLC is disabled by default to ensure backwards compatibility. However, it can be enabled at both the assembly and class level. For more information, see [Compiling XAML](~/xamarin-forms/xaml/xamlc.md).

<a name="correctlayout" />

## Choose the Correct Layout

A layout that's capable of displaying multiple children, but that only has a single child, is wasteful. For example, the following code example shows a [`StackLayout`](xref:Xamarin.Forms.StackLayout) with a single child:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <StackLayout>
            <Image Source="waterfront.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

This is wasteful and the [`StackLayout`](xref:Xamarin.Forms.StackLayout) element should be removed, as shown in the following code example:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DisplayImage.HomePage">
    <ContentPage.Content>
        <Image Source="waterfront.jpg" />
    </ContentPage.Content>
</ContentPage>
```

In addition, don't attempt to reproduce the appearance of a specific layout by using combinations of other layouts, as this results in unnecessary layout calculations being performed. For example, don't attempt to reproduce a [`Grid`](xref:Xamarin.Forms.Grid) layout by using a combination of [`StackLayout`](xref:Xamarin.Forms.StackLayout) instances. The following code example shows an example of this bad practice:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
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
    </ContentPage.Content>
</ContentPage>
```

This is wasteful because unnecessary layout calculations are performed. Instead, the desired layout can be better achieved using a [`Grid`](xref:Xamarin.Forms.Grid), as shown in the following code example:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Details.HomePage"
             Padding="0,20,0,0">
    <ContentPage.Content>
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
    </ContentPage.Content>
</ContentPage>
```

<a name="layoutcompression" />

## Enable Layout Compression

Layout compression removes specified layouts from the visual tree, in an attempt to improve page rendering performance. The performance benefit that this delivers varies depending on the complexity of a page, the version of the operating system being used, and the device on which the application is running. However, the biggest performance gains will be seen on older devices. For more information, see [Layout Compression](~/xamarin-forms/user-interface/layouts/layout-compression.md).

<a name="fastrenderers" />

## Use Fast Renderers

Fast renderers reduce the inflation and rendering costs of Xamarin.Forms controls on Android by flattening the resulting native control hierarchy. This further improves performance by creating fewer objects, which in turns results in a less complex visual tree, and less memory use. For more information, see [Fast Renderers](~/xamarin-forms/internals/fast-renderers.md).

<a name="databinding" />

## Reduce Unnecessary Bindings

Don't use bindings for content that can easily be set statically. There is no advantage in binding data that doesn't need to be bound, because bindings aren't cost efficient. For example, setting `Button.Text = "Accept"` has less overhead than binding [`Button.Text`](xref:Xamarin.Forms.Button.Text) to a ViewModel `string` property with value "Accept".

<a name="optimizelayout" />

## Optimize Layout Performance

Xamarin.Forms 2 introduced an optimized layout engine that impacts layout updates. To obtain the best possible layout performance, follow these guidelines:

- Reduce the depth of layout hierarchies by specifying [`Margin`](xref:Xamarin.Forms.View.Margin) property values, allowing the creation of layouts with fewer wrapping views. For more information, see [Margins and Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).
- When using a [`Grid`](xref:Xamarin.Forms.Grid), try to ensure that as few rows and columns as possible are set to [`Auto`](xref:Xamarin.Forms.GridLength.Auto) size. Each auto-sized row or column will cause the layout engine to perform additional layout calculations. Instead, use fixed size rows and columns if possible. Alternatively, set rows and columns to occupy a proportional amount of space with the [`GridUnitType.Star`](xref:Xamarin.Forms.GridUnitType.Star) enumeration value, provided that the parent tree follows these layout guidelines.
- Don't set the [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) and [`HorizontalOptions`](xref:Xamarin.Forms.View.VerticalOptions) properties of a layout unless required. The default values of [`LayoutOptions.Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) and [`LayoutOptions.FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) allow for the best layout optimization. Changing these properties has a cost and consumes memory, even when setting them to the default values.
- Avoid using a [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout) whenever possible. It will result in the CPU having to perform significantly more work.
- When using an [`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout), avoid using the [`AbsoluteLayout.AutoSize`](xref:Xamarin.Forms.AbsoluteLayout.AutoSize) property whenever possible.
- When using a [`StackLayout`](xref:Xamarin.Forms.StackLayout), ensure that only one child is set to [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands). This property ensures that the specified child will occupy the largest space that the `StackLayout` can give to it, and it is wasteful to perform these calculations more than once.
- Don't call any of the methods of the [`Layout`](xref:Xamarin.Forms.Layout) class, as they result in expensive layout calculations being performed. Instead, it's likely that the desired layout behavior can be obtained by setting the [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) and [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) properties. Alternatively, subclass the [`Layout<View>`](xref:Xamarin.Forms.Layout`1) class to achieve the desired layout behavior.
- Don't update any [`Label`](xref:Xamarin.Forms.Label) instances more frequently than required, as the change of size of the label can result in the entire screen layout being re-calculated.
- Don't set the [`Label.VerticalTextAlignment`](xref:Xamarin.Forms.Label.VerticalTextAlignment) property unless required.
- Set the [`LineBreakMode`](xref:Xamarin.Forms.Label.LineBreakMode) of any [`Label`](xref:Xamarin.Forms.Label) instances to [`NoWrap`](xref:Xamarin.Forms.LineBreakMode.NoWrap) whenever possible.

<a name="optimizelistview" />

## Optimize ListView Performance

When using a [`ListView`](xref:Xamarin.Forms.ListView) control there are a number of user experiences that should be optimized:

- **Initialization** – the time interval starting when the control is created, and ending when items are shown on screen.
- **Scrolling** – the ability to scroll through the list and ensure that the UI doesn't lag behind touch gestures.
- **Interaction** for adding, deleting, and selecting items.

The [`ListView`](xref:Xamarin.Forms.ListView) control requires an application to supply data and cell templates. How this is achieved will have a large impact on the performance of the control. For more information, see [ListView Performance](~/xamarin-forms/user-interface/listview/performance.md).

<a name="optimizeimages" />

## Optimize Image Resources

Displaying image resources can greatly increase the app's memory footprint. Therefore, they should only be created when required and should be released as soon as the application no longer requires them. For example, if an application is displaying an image by reading its data from a stream, ensure that stream is created only when it's required, and ensure that the stream is released when it's no longer required. This can be achieved by creating the stream when the page is created, or when the [`Page.Appearing`](xref:Xamarin.Forms.Page.Appearing) event fires, and then disposing of the stream when the [`Page.Disappearing`](xref:Xamarin.Forms.Page.Disappearing) event fires.

When downloading an image for display with the [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) method, cache the downloaded image by ensuring that the [`UriImageSource.CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) property is set to `true`. For more information, see [Working with Images](~/xamarin-forms/user-interface/images.md).

For more information, see [Optimize Image Resources](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages).

<a name="visualtree" />

## Reduce the Visual Tree Size

Reducing the number of elements on a page will make the page render faster. There are two main techniques for achieving this. The first is to hide elements that aren't visible. The [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) property of each element determines whether the element should be part of the visual tree or not. Therefore, if an element isn't visible because it's hidden behind other elements, either remove the element or set its `IsVisible` property to `false`.

The second technique is to remove unnecessary elements. For example, the following code example shows a page layout that displays a series of [`Label`](xref:Xamarin.Forms.Label) elements:

```xaml
<ContentPage.Content>
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
</ContentPage.Content>
```

The same page layout can be maintained with a reduced element count, as shown in the following code example:

```xaml
<ContentPage.Content>
  <StackLayout Padding="20,20,0,0" Spacing="25">
    <Label Text="Hello" />
    <Label Text="Welcome to the App!" />
    <Label Text="Downloading Data..." />
  </StackLayout>
</ContentPage.Content>
```

<a name="resourcedictionary" />

## Reduce the Application Resource Dictionary Size

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

However, XAML that's specific to a page shouldn't be included in the app's resource dictionary, as the resources will then be parsed at application startup instead of when required by a page. If a resource is used by a page that's not the startup page, it should be placed in the resource dictionary for that page, therefore helping to reduce the XAML that's parsed when the application starts. The following code example shows the `HeadingLabelStyle` resource, which is only on a single page, and so is defined in the page's resource dictionary:

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
    <ContentPage.Content>
      ...
    </ContentPage.Content>
</ContentPage>

```

For more information about application resources, see [`Working with Styles`](~/xamarin-forms/user-interface/styles/index.md).

<a name="rendererpattern" />

## Use the Custom Renderer Pattern

Most renderer classes expose the `OnElementChanged` method, which is called when a Xamarin.Forms custom control is created to render the corresponding native control. Custom renderer classes, in each platform-specific renderer class, then override this method to instantiate and customize the native control. The `SetNativeControl` method is used to instantiate the native control, and this method will also assign the control reference to the `Control` property.

However, in some circumstances the `OnElementChanged` method can be called multiple times. Therefore, to prevent memory leaks, which can have a performance impact, care must be taken when instantiating a new native control. The approach to use when instantiating a new native control in a custom renderer is shown in the following code example:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

A new native control should only be instantiated once, when the `Control` property is `null`. The control should only be configured and event handlers subscribed to when the custom renderer is attached to a new Xamarin.Forms element. Similarly, any event handlers that were subscribed to should only be unsubscribed from when the element renderer is attached to changes. Adopting this approach will help to create an efficiently performing custom renderer that doesn't suffer from memory leaks.

For more information about custom renderers, see [Customizing Controls on Each Platform](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

## Summary

This article described and discussed techniques for increasing the performance of Xamarin.Forms applications. Collectively these techniques can greatly reduce the amount of work being performed by a CPU, and the amount of memory consumed by an application.


## Related Links

- [Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [ListView Performance](~/xamarin-forms/user-interface/listview/performance.md)
- [Fast Renderers](~/xamarin-forms/internals/fast-renderers.md)
- [Layout Compression](~/xamarin-forms/user-interface/layouts/layout-compression.md)
- [XamlCompilation](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [XamlCompilationOptions](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
