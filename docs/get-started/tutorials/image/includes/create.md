# [Visual Studio](#tab/vswin)

To complete this tutorial you should have Visual Studio 2019 (latest release), with the **Mobile development with .NET** workload installed. In addition, you will require a paired Mac to build the tutorial application on iOS. For information about installing the Xamarin platform, see [Installing Xamarin](~/get-started/installation/index.md). For information about connecting Visual Studio 2019 to a Mac build host, see [Pair to Mac for Xamarin.iOS development](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Launch Visual Studio, and create a new blank Xamarin.Forms app named **ImageTutorial**.

    > [!IMPORTANT]
    > The C# and XAML snippets in this tutorial requires that the solution is named **ImageTutorial**. Using a different name will result in build errors when you copy code from this tutorial into the solution.

    For more information about the .NET Standard library that gets created, see [Anatomy of a Xamarin.Forms application](~/get-started/first-app/index.md) in the [Xamarin.Forms Quickstart Deep Dive](~/get-started/first-app/index.md).

1. In **Solution Explorer**, in the **ImageTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of an [`Image`](xref:Xamarin.Forms.Image) in a [`StackLayout`](xref:Xamarin.Forms.StackLayout). The [`Image.Source`](xref:Xamarin.Forms.Image.Source) property specifies the image to display, via a URI. The [`Image.Source`](xref:Xamarin.Forms.Image.Source) property is of type [`ImageSource`](xref:Xamarin.Forms.ImageSource), which enables images to be sourced from files, URIs, or resources. For more information, see [Displaying images](~/xamarin-forms/user-interface/images.md#display-images) in the [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) guide.

    The [`HeightRequest`](xref:Xamarin.Forms.VisualElement) property specifies the height of the `Image` in device-independent units.

    > [!NOTE]
    > It's not necessary to set the [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) property in this example. This is because, by default, the [`Image`](xref:Xamarin.Forms.Image) maintains the aspect ratio of the image.

1. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator:

    [![Screenshot of an Image on iOS and Android](../images/create-image.png "Image view displaying an image")](../images/create-image-large.png#lightbox "Image view displaying an image")

    > [!NOTE]
    > The [`Image`](xref:Xamarin.Forms.Image) view automatically caches downloaded images for 24 hours. For more information, see [Downloaded image caching](~/xamarin-forms/user-interface/images.md#downloaded-image-caching) in the [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) guide.

# [Visual Studio for Mac](#tab/vsmac)

To complete this tutorial you should have Visual Studio for Mac (latest release), with iOS and Android platform support installed. In addition, you will also require Xcode (latest release). For more information about installing the Xamarin platform, see [Installing Xamarin](~/get-started/installation/index.md).

1. Launch Visual Studio for Mac, and create a new blank Xamarin.Forms app named **ImageTutorial**.

    > [!IMPORTANT]
    > The C# and XAML snippets in this tutorial requires that the solution is named **ImageTutorial**. Using a different name will result in build errors when you copy code from this tutorial into the solution.

    For more information about the .NET Standard library that gets created, see [Anatomy of a Xamarin.Forms application](~/get-started/first-app/index.md) in the [Xamarin.Forms Quickstart Deep Dive](~/get-started/first-app/index.md).

1. In **Solution Pad**, in the **ImageTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ImageTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Image Source="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Papio_anubis_%28Serengeti%2C_2009%29.jpg/200px-Papio_anubis_%28Serengeti%2C_2009%29.jpg"
                   HeightRequest="300" />
        </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of an [`Image`](xref:Xamarin.Forms.Image) in a [`StackLayout`](xref:Xamarin.Forms.StackLayout). The [`Image.Source`](xref:Xamarin.Forms.Image.Source) property specifies the image to display, via a URI. The [`Image.Source`](xref:Xamarin.Forms.Image.Source) property is of type [`ImageSource`](xref:Xamarin.Forms.ImageSource), which enables images to be sourced from files, URIs, or resources. For more information, see [Displaying images](~/xamarin-forms/user-interface/images.md#display-images) in the [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) guide.

    The [`HeightRequest`](xref:Xamarin.Forms.VisualElement) property specifies the height of the `Image` in device-independent units.

    > [!NOTE]
    > It's not necessary to set the [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) property in this example. This is because, by default, the [`Image`](xref:Xamarin.Forms.Image) maintains the aspect ratio of the image.

1. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator:

    [![Screenshot of an Image on iOS and Android](../images/create-image.png "Image view displaying an image")](../images/create-image-large.png#lightbox "Image view displaying an image")

    > [!NOTE]
    > The [`Image`](xref:Xamarin.Forms.Image) view automatically caches downloaded images for 24 hours. For more information, see [Downloaded image caching](~/xamarin-forms/user-interface/images.md#downloaded-image-caching) in the [Images in Xamarin.Forms](~/xamarin-forms/user-interface/images.md) guide.
