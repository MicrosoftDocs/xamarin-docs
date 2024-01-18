---
title: "Xamarin.Android TextureView"
description: The TextureView class is a view that uses hardware-accelerated 2D rendering to enable a video or OpenGL content stream to be displayed.
ms.service: xamarin
ms.assetid: DD1F3D68-5DD8-4644-8A13-08AE7719DE30
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2017
---

# Xamarin.Android TextureView

The `TextureView` class is a view that uses hardware-accelerated
2D rendering to enable a video or OpenGL content stream to be displayed. For
example, the following screenshot shows the `TextureView` displaying
a live feed from the device's camera:

[![Example screenshot of a live image from the device's camera](texture-view-images/22-textureviewcamera.png)](texture-view-images/22-textureviewcamera.png#lightbox)

Unlike the `SurfaceView` class, which can also be used to display
OpenGL or video content, the TextureView is not rendered into a separate window.
Therefore, `TextureView` is able to support view transformations like
any other view. For example, rotating a `TextureView` can be
accomplished by simply setting its `Rotation` property, its
transparency by setting its `Alpha` property, and so on.

Therefore, with the `TextureView` we can now do things like
display a live stream from the camera and transform it, as shown in the
following code:

```csharp
public class TextureViewActivity : Activity,
    TextureView.ISurfaceTextureListener
{
    Camera _camera;
    TextureView _textureView;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        _textureView = new TextureView (this);
        _textureView.SurfaceTextureListener = this;

        SetContentView (_textureView);
    }

    public void OnSurfaceTextureAvailable (
        Android.Graphics.SurfaceTexture surface,
        int width, int height)
    {
        _camera = Camera.Open ();
        var previewSize = _camera.GetParameters ().PreviewSize;
        _textureView.LayoutParameters =
            new FrameLayout.LayoutParams (previewSize.Width,
                previewSize.Height, (int)GravityFlags.Center);

        try {
            _camera.SetPreviewTexture (surface);
            _camera.StartPreview ();
        } catch (Java.IO.IOException ex) {
            Console.WriteLine (ex.Message);
        }

        // this is the sort of thing TextureView enables
        _textureView.Rotation = 45.0f;
        _textureView.Alpha = 0.5f;
    }
    …
}
```

The above code creates a `TextureView` instance in the Activity's
`OnCreate` method and sets the Activity as the `TextureView`'s
`SurfaceTextureListener`. To be the `SurfaceTextureListener`, the
Activity implements the `TextureView.ISurfaceTextureListener`
interface. The system will call the `OnSurfaceTextAvailable` method
when the `SurfaceTexture` is ready for use. In this method, we take the
`SurfaceTexture` that is passed in and set it to the camera's preview
texture. Then we are free to perform normal view-based operations, such
as setting the `Rotation` and `Alpha`, as in the example above. The
resulting application, running on a device, is shown below:

[![Example of the app running on a device, displaying an image](texture-view-images/17-textureviewdemo.png)](texture-view-images/17-textureviewdemo.png#lightbox)

To use the `TextureView`, hardware acceleration must be enabled, which
it will be by default as of API Level 14. Also, since this example uses
the camera, both the `android.permission.CAMERA` permission and the
`android.hardware.camera` feature must be set in the
**AndroidManifest.xml**.

## Related Links

- [TextureViewDemo (sample)](/samples/xamarin/monodroid-samples/textureviewdemo)/)