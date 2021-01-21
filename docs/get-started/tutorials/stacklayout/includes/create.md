A [`StackLayout`](xref:Xamarin.Forms.StackLayout) is a layout that organizes its children in a one-dimensional stack, either horizontally or vertically. By default, a `StackLayout` is oriented vertically.

# [Visual Studio](#tab/vswin)

To complete this tutorial you should have Visual Studio 2019 (latest release), with the **Mobile development with .NET** workload installed. In addition, you will require a paired Mac to build the tutorial application on iOS. For information about installing the Xamarin platform, see [Installing Xamarin](~/get-started/installation/index.md). For information about connecting Visual Studio 2019 to a Mac build host, see [Pair to Mac for Xamarin.iOS development](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Launch Visual Studio, and create a new blank Xamarin.Forms app named **StackLayoutTutorial**.

    > [!IMPORTANT]
    > The C# and XAML snippets in this tutorial requires that the solution is named **StackLayoutTutorial**. Using a different name will result in build errors when you copy code from this tutorial into the solution.

    For more information about the .NET Standard library that gets created, see [Anatomy of a Xamarin.Forms application](~/get-started/quickstarts/deepdive.md#anatomy-of-a-xamarinforms-application) in the [Xamarin.Forms Quickstart Deep Dive](~/get-started/quickstarts/deepdive.md).

1. In **Solution Explorer**, in the **StackLayoutTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of three [`Label`](xref:Xamarin.Forms.Label) instances in a [`StackLayout`](xref:Xamarin.Forms.StackLayout). The [`StackLayout`](xref:Xamarin.Forms.StackLayout) positions its child views (the `Label` instances) in a single line, which is oriented vertically by default. In addition, the [`Margin`](xref:Xamarin.Forms.View.Margin) property indicates the rendering position of the `StackLayout` within the [`ContentPage`](xref:Xamarin.Forms.ContentPage).

    > [!NOTE]
    > In addition to the [`Margin`](xref:Xamarin.Forms.View.Margin) property, [`Padding`](xref:Xamarin.Forms.Layout.Padding) and [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) properties can also be set on a [`StackLayout`](xref:Xamarin.Forms.StackLayout). The [`Padding`](xref:Xamarin.Forms.Layout.Padding) property value specifies the distance between views in the `StackLayout`, and the [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) property value specifies the amount of space between each child element in the `StackLayout`. For more information, see [Margin and Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator:

    [![Screenshot of child views in a StackLayout, on iOS and Android](../images/create-stacklayout.png "StackLayout containing Label instances")](../images/create-stacklayout-large.png#lightbox "StackLayout containing Label instances")

    For more information about the [`StackLayout`](xref:Xamarin.Forms.StackLayout), see [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stacklayout.md).

# [Visual Studio for Mac](#tab/vsmac)

To complete this tutorial you should have Visual Studio for Mac (latest release), with iOS and Android platform support installed. In addition, you will also require Xcode (latest release). For more information about installing the Xamarin platform, see [Installing Xamarin](~/get-started/installation/index.md).

1. Launch Visual Studio for Mac, and create a new blank Xamarin.Forms app named **StackLayoutTutorial**.

    > [!IMPORTANT]
    > The C# and XAML snippets in this tutorial requires that the solution is named **StackLayoutTutorial**. Using a different name will result in build errors when you copy code from this tutorial into the solution.

    For more information about the .NET Standard library that gets created, see [Anatomy of a Xamarin.Forms application](~/get-started/first-app/index.md) in the [Xamarin.Forms Quickstart Deep Dive](~/get-started/first-app/index.md).

1. In **Solution Pad**, in the **StackLayoutTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="StackLayoutTutorial.MainPage">
        <StackLayout Margin="20,35,20,25">
            <Label Text="The StackLayout has its Margin property set, to control the rendering position of the StackLayout." />
            <Label Text="The Padding property can be set to specify the distance between the StackLayout and its children." />
            <Label Text="The Spacing property can be set to specify the distance between views in the StackLayout." />
        </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of three [`Label`](xref:Xamarin.Forms.Label) instances in a [`StackLayout`](xref:Xamarin.Forms.StackLayout). The [`StackLayout`](xref:Xamarin.Forms.StackLayout) positions its child views (the `Label` instances) in a single line, which is oriented vertically by default. In addition, the [`Margin`](xref:Xamarin.Forms.View.Margin) property indicates the rendering position of the `StackLayout` within the [`ContentPage`](xref:Xamarin.Forms.ContentPage).

    > [!NOTE]
    > In addition to the [`Margin`](xref:Xamarin.Forms.View.Margin) property, [`Padding`](xref:Xamarin.Forms.Layout.Padding) and [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) properties can also be set on a [`StackLayout`](xref:Xamarin.Forms.StackLayout). The [`Padding`](xref:Xamarin.Forms.Layout.Padding) property value specifies the distance between views in the `StackLayout`, and the [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) property value specifies the amount of space between each child element in the `StackLayout`. For more information, see [Margin and Padding](~/xamarin-forms/user-interface/layouts/margin-and-padding.md).

1. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator:

    [![Screenshot of child views in a StackLayout, on iOS and Android](../images/create-stacklayout.png "StackLayout containing Label instances")](../images/create-stacklayout-large.png#lightbox "StackLayout containing Label instances")

    For more information about the [`StackLayout`](xref:Xamarin.Forms.StackLayout), see [Xamarin.Forms StackLayout](~/xamarin-forms/user-interface/layouts/stacklayout.md).
