# [Visual Studio](#tab/vswin)

To complete this tutorial you should have Visual Studio 2019 (latest release), with the **Mobile development with .NET** workload installed. In addition, you will require a paired Mac to build the tutorial application on iOS. For information about installing the Xamarin platform, see [Installing Xamarin](~/get-started/installation/index.md). For information about connecting Visual Studio 2019 to a Mac build host, see [Pair to Mac for Xamarin.iOS development](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Launch Visual Studio, and create a new blank Xamarin.Forms app named **ButtonTutorial**.

    > [!IMPORTANT]
    > The C# and XAML snippets in this tutorial requires that the solution is named **ButtonTutorial**. Using a different name will result in build errors when you copy code from this tutorial into the solution.

    For more information about the .NET Standard library that gets created, see [Anatomy of a Xamarin.Forms application](~/get-started/first-app/index.md) in the [Xamarin.Forms Quickstart Deep Dive](~/get-started/first-app/index.md).

1. In **Solution Explorer**, in the **ButtonTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>    
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of a [`Button`](xref:Xamarin.Forms.Button) in a [`StackLayout`](xref:Xamarin.Forms.StackLayout). The [`Button.Text`](xref:Xamarin.Forms.Button.Text) property specifies the text that appears in the `Button`.

1. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator:

    [![Screenshot of a Button on iOS and Android](../images/create-button.png "Button containing text")](../images/create-button-large.png#lightbox "Button containing text")

    Note that by default a [`Button`](xref:Xamarin.Forms.Button) tends to occupy all the space that's allowed for it - in this case, the full width of its parent (the [`StackLayout`](xref:Xamarin.Forms.StackLayout)).

    In Visual Studio, stop the application.

# [Visual Studio for Mac](#tab/vsmac)

To complete this tutorial you should have Visual Studio for Mac (latest release), with iOS and Android platform support installed. In addition, you will also require Xcode (latest release). For more information about installing the Xamarin platform, see [Installing Xamarin](~/get-started/installation/index.md).

1. Launch Visual Studio for Mac, and create a new blank Xamarin.Forms app named **ButtonTutorial**.

    > [!IMPORTANT]
    > The C# and XAML snippets in this tutorial requires that the solution is named **ButtonTutorial**. Using a different name will result in build errors when you copy code from this tutorial into the solution.

    For more information about the .NET Standard library that gets created, see [Anatomy of a Xamarin.Forms application](~/get-started/first-app/index.md) in the [Xamarin.Forms Quickstart Deep Dive](~/get-started/first-app/index.md).

1. In **Solution Pad**, in the **ButtonTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="ButtonTutorial.MainPage">
       <StackLayout Margin="20,35,20,20">
           <Button Text="Click me" />
       </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of a [`Button`](xref:Xamarin.Forms.Button) in a [`StackLayout`](xref:Xamarin.Forms.StackLayout). The [`Button.Text`](xref:Xamarin.Forms.Button.Text) property specifies the text that appears in the `Button`.

1. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator:

    [![Screenshot of a Button on iOS and Android](../images/create-button.png "Button containing text")](../images/create-button-large.png#lightbox "Button containing text")

    Note that by default a [`Button`](xref:Xamarin.Forms.Button) tends to occupy all the space that's allowed for it - in this case, the full width of its parent (the [`StackLayout`](xref:Xamarin.Forms.StackLayout)).

    In Visual Studio for Mac, stop the application.
