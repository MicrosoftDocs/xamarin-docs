---
title: "Implementing a View"
description: "This article explains how to create a custom renderer for a Xamarin.Forms custom control that's used to display a preview video stream from the device's camera."
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
---
# Implementing a View

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)

_Xamarin.Forms custom user interface controls should derive from the View class, which is used to place layouts and controls on the screen. This article demonstrates how to create a custom renderer for a Xamarin.Forms custom control that's used to display a preview video stream from the device's camera._

Every Xamarin.Forms view has an accompanying renderer for each platform that creates an instance of a native control. When a [`View`](xref:Xamarin.Forms.View) is rendered by a Xamarin.Forms application in iOS, the `ViewRenderer` class is instantiated, which in turn instantiates a native `UIView` control. On the Android platform, the `ViewRenderer` class instantiates a native `View` control. On the Universal Windows Platform (UWP), the `ViewRenderer` class instantiates a native `FrameworkElement` control. For more information about the renderer and native control classes that Xamarin.Forms controls map to, see [Renderer Base Classes and Native Controls](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

The following diagram illustrates the relationship between the [`View`](xref:Xamarin.Forms.View) and the corresponding native controls that implement it:

![](view-images/view-classes.png "Relationship Between the View Class and its Implementing Native Classes")

The rendering process can be used to implement platform-specific customizations by creating a custom renderer for a [`View`](xref:Xamarin.Forms.View) on each platform. The process for doing this is as follows:

1. [Create](#Creating_the_Custom_Control) a Xamarin.Forms custom control.
1. [Consume](#Consuming_the_Custom_Control) the custom control from Xamarin.Forms.
1. [Create](#Creating_the_Custom_Renderer_on_each_Platform) the custom renderer for the control on each platform.

Each item will now be discussed in turn, to implement a `CameraPreview` renderer that displays a preview video stream from the device's camera. Tapping on the video stream will stop and start it.

<a name="Creating_the_Custom_Control" />

## Creating the Custom Control

A custom control can be created by subclassing the [`View`](xref:Xamarin.Forms.View) class, as shown in the following code example:

```csharp
public class CameraPreview : View
{
  public static readonly BindableProperty CameraProperty = BindableProperty.Create (
    propertyName: "Camera",
    returnType: typeof(CameraOptions),
    declaringType: typeof(CameraPreview),
    defaultValue: CameraOptions.Rear);

  public CameraOptions Camera {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

The `CameraPreview` custom control is created in the portable class library (PCL) project and defines the API for the control. The custom control exposes a `Camera` property that's used for controlling whether the video stream should be displayed from the front or rear camera on the device. If a value isn't specified for the `Camera` property when the control is created, it defaults to specifying the rear camera.

<a name="Consuming_the_Custom_Control" />

## Consuming the Custom Control

The `CameraPreview` custom control can be referenced in XAML in the PCL project by declaring a namespace for its location and using the namespace prefix on the custom control element. The following code example shows how the `CameraPreview` custom control can be consumed by a XAML page:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Camera Preview:" />
            <local:CameraPreview Camera="Rear"
              HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

The `local` namespace prefix can be named anything. However, the `clr-namespace` and `assembly` values must match the details of the custom control. Once the namespace is declared, the prefix is used to reference the custom control.

The following code example shows how the `CameraPreview` custom control can be consumed by a C# page:

```csharp
public class MainPageCS : ContentPage
{
  public MainPageCS ()
  {
    ...
    Content = new StackLayout {
      Children = {
        new Label { Text = "Camera Preview:" },
        new CameraPreview {
          Camera = CameraOptions.Rear,
          HorizontalOptions = LayoutOptions.FillAndExpand,
          VerticalOptions = LayoutOptions.FillAndExpand
        }
      }
    };
  }
}
```

An instance of the `CameraPreview` custom control will be used to display the preview video stream from the device's camera. Aside from optionally specifying a value for the `Camera` property, customization of the control will be carried out in the custom renderer.

A custom renderer can now be added to each application project to create platform-specific camera preview controls.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## Creating the Custom Renderer on each Platform

The process for creating the custom renderer class is as follows:

1. Create a subclass of the `ViewRenderer<T1,T2>` class that renders the custom control. The first type argument should be the custom control the renderer is for, in this case `CameraPreview`. The second type argument should be the native control that will implement the custom control.
1. Override the `OnElementChanged` method that renders the custom control and write logic to customize it. This method is called when the corresponding Xamarin.Forms control is created.
1. Add an `ExportRenderer` attribute to the custom renderer class to specify that it will be used to render the Xamarin.Forms custom control. This attribute is used to register the custom renderer with Xamarin.Forms.

> [!NOTE]
> For most Xamarin.Forms elements, it is optional to provide a custom renderer in each platform project. If a custom renderer isn't registered, then the default renderer for the control's base class will be used. However, custom renderers are required in each platform project when rendering a [View](xref:Xamarin.Forms.View) element.

The following diagram illustrates the responsibilities of each project in the sample application, along with the relationships between them:

![](view-images/solution-structure.png "CameraPreview Custom Renderer Project Responsibilities")

The `CameraPreview` custom control is rendered by platform-specific renderer classes, which all derive from the `ViewRenderer` class for each platform. This results in each `CameraPreview` custom control being rendered with platform-specific controls, as shown in the following screenshots:

![](view-images/screenshots.png "CameraPreview on each Platform")

The `ViewRenderer` class exposes the `OnElementChanged` method, which is called when the Xamarin.Forms custom control is created to render the corresponding native control. This method takes an `ElementChangedEventArgs` parameter that contains `OldElement` and `NewElement` properties. These properties represent the Xamarin.Forms element that the renderer *was* attached to, and the Xamarin.Forms element that the renderer *is* attached to, respectively. In the sample application, the `OldElement` property will be `null` and the `NewElement` property will contain a reference to the `CameraPreview` instance.

An overridden version of the `OnElementChanged` method, in each platform-specific renderer class, is the place to perform the native control instantiation and customization. The `SetNativeControl` method should be used to instantiate the native control, and this method will also assign the control reference to the `Control` property. In addition, a reference to the Xamarin.Forms control that's being rendered can be obtained through the `Element` property.

In some circumstances, the `OnElementChanged` method can be called multiple times. Therefore, to prevent memory leaks, care must be taken when instantiating a new native control. The approach to use when instantiating a new native control in a custom renderer is shown in the following code example:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

A new native control should only be instantiated once, when the `Control` property is `null`. The control should only be configured and event handlers subscribed to when the custom renderer is attached to a new Xamarin.Forms element. Similarly, any event handlers that were subscribed to should only be unsubscribed from when the element that the renderer is attached to changes. Adopting this approach will help to create a performant custom renderer that doesn't suffer from memory leaks.

Each custom renderer class is decorated with an `ExportRenderer` attribute that registers the renderer with Xamarin.Forms. The attribute takes two parameters – the type name of the Xamarin.Forms custom control being rendered, and the type name of the custom renderer. The `assembly` prefix to the attribute specifies that the attribute applies to the entire assembly.

The following sections discuss the implementation of each platform-specific custom renderer class.

### Creating the Custom Renderer on iOS

The following code example shows the custom renderer for the iOS platform:

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, UICameraPreview>
    {
        UICameraPreview uiCameraPreview;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                SetNativeControl (uiCameraPreview);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                uiCameraPreview.Tapped += OnCameraPreviewTapped;
            }
        }

        void OnCameraPreviewTapped (object sender, EventArgs e)
        {
            if (uiCameraPreview.IsPreviewing) {
                uiCameraPreview.CaptureSession.StopRunning ();
                uiCameraPreview.IsPreviewing = false;
            } else {
                uiCameraPreview.CaptureSession.StartRunning ();
                uiCameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Provided that the `Control` property is `null`, the `SetNativeControl` method is called to instantiate a new `UICameraPreview` control and to assign a reference to it to the `Control` property. The `UICameraPreview` control is a platform-specific custom control that uses the `AVCapture` APIs to provide the preview stream from the camera. It exposes a `Tapped` event that's handled by the `OnCameraPreviewTapped` method to stop and start the video preview when it's tapped. The `Tapped` event is subscribed to when the custom renderer is attached to a new Xamarin.Forms element, and unsubscribed from only when the element the renderer is attached to changes.

### Creating the Custom Renderer on Android

The following code example shows the custom renderer for the Android platform:

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : ViewRenderer<CustomRenderer.CameraPreview, CustomRenderer.Droid.CameraPreview>
    {
        CameraPreview cameraPreview;

        public CameraPreviewRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<CustomRenderer.CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                cameraPreview = new CameraPreview(Context);
                SetNativeControl(cameraPreview);
            }

            if (e.OldElement != null)
            {
                // Unsubscribe
                cameraPreview.Click -= OnCameraPreviewClicked;
            }
            if (e.NewElement != null)
            {
                Control.Preview = Camera.Open((int)e.NewElement.Camera);

                // Subscribe
                cameraPreview.Click += OnCameraPreviewClicked;
            }
        }

        void OnCameraPreviewClicked(object sender, EventArgs e)
        {
            if (cameraPreview.IsPreviewing)
            {
                cameraPreview.Preview.StopPreview();
                cameraPreview.IsPreviewing = false;
            }
            else
            {
                cameraPreview.Preview.StartPreview();
                cameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Provided that the `Control` property is `null`, the `SetNativeControl` method is called to instantiate a new `CameraPreview` control and assign a reference to it to the `Control` property. The `CameraPreview` control is a platform-specific custom control that uses the `Camera` API to provide the preview stream from the camera. The `CameraPreview` control is then configured, provided that the custom renderer is attached to a new Xamarin.Forms element. This configuration involves creating a new native `Camera` object to access a particular hardware camera, and registering an event handler to process the `Click` event. In turn this handler will stop and start the video preview when it's tapped. The `Click` event is unsubscribed from if the Xamarin.Forms element the renderer is attached to changes.

### Creating the Custom Renderer on UWP

The following code example shows the custom renderer for UWP:

```csharp
[assembly: ExportRenderer(typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        ...
        CaptureElement _captureElement;
        bool _isPreviewing;

        protected override void OnElementChanged(ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                ...
                _captureElement = new CaptureElement();
                _captureElement.Stretch = Stretch.UniformToFill;

                SetupCamera();
                SetNativeControl(_captureElement);
            }
            if (e.OldElement != null)
            {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
                ...
            }
            if (e.NewElement != null)
            {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped(object sender, TappedRoutedEventArgs e)
        {
            if (_isPreviewing)
            {
                await StopPreviewAsync();
            }
            else
            {
                await StartPreviewAsync();
            }
        }
        ...
    }
}
```

Provided that the `Control` property is `null`, a new `CaptureElement` is instantiated and the `SetupCamera` method is called, which uses the `MediaCapture` API to provide the preview stream from the camera. The `SetNativeControl` method is then called to assign a reference to the `CaptureElement` instance to the `Control` property. The `CaptureElement` control exposes a `Tapped` event that's handled by the `OnCameraPreviewTapped` method to stop and start the video preview when it's tapped. The `Tapped` event is subscribed to when the custom renderer is attached to a new Xamarin.Forms element, and unsubscribed from only when the element the renderer is attached to changes.

> [!NOTE]
> It's important to stop and dispose of the objects that provide access to the camera in a UWP application. Failure to do so can interfere with other applications that attempt to access the device's camera. For more information, see [Display the camera preview](/windows/uwp/audio-video-camera/simple-camera-preview-access/).

## Summary

This article has demonstrated how to create a custom renderer for a Xamarin.Forms custom control that's used to display a preview video stream from the device's camera. Xamarin.Forms custom user interface controls should derive from the [`View`](xref:Xamarin.Forms.View) class, which is used to place layouts and controls on the screen.


## Related Links

- [CustomRendererView (sample)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
