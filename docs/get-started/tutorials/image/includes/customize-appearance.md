# [Visual Studio](#tab/vswin)

1. In **MainPage.xaml**, modify the [`Image`](xref:Xamarin.Forms.Image) declaration to customize its appearance:

    ```xaml
    <Image Source="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    This code sets the [`Aspect`](xref:Xamarin.Forms.Image.Aspect) property, which defines the scaling mode of the image, to [`Fill`](xref:Xamarin.Forms.Aspect.Fill). The `Fill` member is defined in the [`Aspect`](xref:Xamarin.Forms.Aspect) enumeration, and stretches the image to completely fill the view, regardless of whether the image is distorted. For more information about image scaling, see [Displaying images](~/xamarin-forms/user-interface/images.md#display-images) in the [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) guide.

    The `OnPlatform` markup extension enables you to customize UI appearance on a per-platform basis. In this example, the markup extension is used to set the [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) and [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) properties to 300 device-independent units on iOS and to 250 device-independent units on Android. For more information about the `OnPlatform` markup extension, see [OnPlatform markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension) in the [Consuming XAML Markup Extensions](~/xamarin-forms/xaml/markup-extensions/consuming.md) guide.

    In addition, the [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) property specifies that the image will be horizontally centered.

1. If the application is still running, save the changes to the file and the application user interface will automatically be updated in your simulator or emulator. Otherwise, in the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator:

    [![Screenshot of an Image that's sized differently on iOS and Android](../images/customize-appearance.png "Image sized on a per-platform basis")](../images/customize-appearance-large.png#lightbox "Image sized on a per-platform basis")

    In Visual Studio, stop the application.

# [Visual Studio for Mac](#tab/vsmac)

1. In **MainPage.xaml**, modify the [`Image`](xref:Xamarin.Forms.Image) declaration to customize its appearance:

    ```xaml
    <Image Source="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
           Aspect="Fill"
           HeightRequest="{OnPlatform iOS=300, Android=250}"
           WidthRequest="{OnPlatform iOS=300, Android=250}"
           HorizontalOptions="Center" />
    ```

    This code sets the [`Aspect`](xref:Xamarin.Forms.Image.Aspect) property, which defines the scaling mode of the image, to [`Fill`](xref:Xamarin.Forms.Aspect.Fill). The `Fill` member is defined in the [`Aspect`](xref:Xamarin.Forms.Aspect) enumeration, and stretches the image to completely fill the view, regardless of whether the image is distorted. For more information about image scaling, see [Displaying images](~/xamarin-forms/user-interface/images.md#display-images) in the [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) guide.

    The `OnPlatform` markup extension enables you to customize UI appearance on a per-platform basis. In this example, the markup extension is used to set the [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) and [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) properties to 300 on iOS and to 250 on Android. For more information about the `OnPlatform` markup extension, see [OnPlatform markup extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension) in the [Consuming XAML Markup Extensions](~/xamarin-forms/xaml/markup-extensions/consuming.md) guide.

    In addition, the [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) property specifies that the image will be horizontally centered.

1. If the application is still running, save the changes to the file and the application user interface will automatically be updated in your simulator or emulator. Otherwise, in the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator:

    [![Screenshot of an Image that's sized differently on iOS and Android](../images/customize-appearance.png "Image sized on a per-platform basis")](../images/customize-appearance-large.png#lightbox "Image sized on a per-platform basis")

    In Visual Studio for Mac, stop the application.
