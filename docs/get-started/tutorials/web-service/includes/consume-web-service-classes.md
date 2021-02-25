In this exercise you will create a user interface to consume the `RestService` class, which retrieves .NET repository data from the GitHub web API. The retrieved data will be displayed by a [`CollectionView`](xref:Xamarin.Forms.CollectionView).

# [Visual Studio](#tab/vswin)

1. In **Solution Explorer**, in the **WebServiceTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Get Repository Data"
                    Clicked="OnButtonClicked" />
            <CollectionView x:Name="collectionView">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <StackLayout>
                            <Label Text="{Binding Name}"
                                   FontSize="Medium" />
                            <Label Text="{Binding Description}"
                                   TextColor="Silver"
                                   FontSize="Small" />
                            <Label Text="{Binding GitHubHomeUrl}"
                                   TextColor="Gray"
                                   FontSize="Caption" />
                        </StackLayout>
                    </DataTemplate>
                </CollectionView.ItemTemplate>
            </CollectionView>
        </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of a [`Button`](xref:Xamarin.Forms.Button), and a [`CollectionView`](xref:Xamarin.Forms.CollectionView). The `Button` sets its [`Clicked`](xref:Xamarin.Forms.Button.Clicked) event to an event handler named `OnButtonClicked` that will be created in the next step. The `CollectionView` sets the [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), which defines the appearance of each item in the `CollectionView`. The child of the `DataTemplate` is a [`StackLayout`](xref:Xamarin.Forms.StackLayout), which contains three [`Label`](xref:Xamarin.Forms.Label) objects. The `Label` objects bind their [`Text`](xref:Xamarin.Forms.Label.Text) properties to the `Name`, `Description`, and `GitHubHomeUrl` properties of each `Repository` object. For more information about data binding, see [Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    In addition, the [`CollectionView`](xref:Xamarin.Forms.CollectionView) has a name specified with the `x:Name` attribute. This enables the code-behind file to access the object using the assigned name.

1. In **Solution Explorer**, in the **WebServiceTutorial** project, expand **MainPage.xaml** and double-click **MainPage.xaml.cs** to open it. Then, in **MainPage.xaml.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                List<Repository> repositories = await _restService.GetRepositoriesAsync(Constants.GitHubReposEndpoint);
                collectionView.ItemsSource = repositories;
            }
        }
    }
    ```

    The `OnButtonClicked` method, which is executed when the [`Button`](xref:Xamarin.Forms.Button) is tapped, invokes the `RestService.GetRepositoriesAsync` method to retrieve .NET repository data from the GitHub web API. The `GetRepositoriesAsync` method requires a `string` argument representing the URI of the web API being invoked, and this is returned by the `Constants.GitHubReposEndpoint` field.

    After retrieving the requested data, the [`CollectionView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) property is set to the retrieved data.

1. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator. Tap the [`Button`](xref:Xamarin.Forms.Button) to retrieve .NET repository data from GitHub:

    [![Screenshot of GitHub .NET repositories, on iOS and Android](../images/consume-web-service.png)](../images/consume-web-service-large.png#lightbox)

    In Visual Studio, stop the application.

    For more information about consuming REST-based web services in Xamarin.Forms, see [Consume a RESTful Web Service (guide)](~/xamarin-forms/data-cloud/web-services/rest.md).

# [Visual Studio for Mac](#tab/vsmac)

1. In **Solution Pad**, in the **WebServiceTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Button Text="Get Repository Data"
                    Clicked="OnButtonClicked" />
            <CollectionView x:Name="collectionView">
                <CollectionView.ItemTemplate>
                    <DataTemplate>
                        <StackLayout>
                            <Label Text="{Binding Name}"
                                   FontSize="Medium" />
                            <Label Text="{Binding Description}"
                                   TextColor="Silver"
                                   FontSize="Small" />
                            <Label Text="{Binding GitHubHomeUrl}"
                                   TextColor="Gray"
                                   FontSize="Caption" />
                        </StackLayout>
                    </DataTemplate>
                </CollectionView.ItemTemplate>
            </CollectionView>
        </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of a [`Button`](xref:Xamarin.Forms.Button), and a [`CollectionView`](xref:Xamarin.Forms.CollectionView). The `Button` sets its [`Clicked`](xref:Xamarin.Forms.Button.Clicked) event to an event handler named `OnButtonClicked` that will be created in the next step. The `CollectionView` sets the [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), which defines the appearance of each item in the `CollectionView`. The child of the `DataTemplate` is a [`StackLayout`](xref:Xamarin.Forms.StackLayout), which contains three [`Label`](xref:Xamarin.Forms.Label) objects. The `Label` objects bind their [`Text`](xref:Xamarin.Forms.Label.Text) properties to the `Name`, `Description`, and `GitHubHomeUrl` properties of each `Repository` object. For more information about data binding, see [Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    In addition, the [`CollectionView`](xref:Xamarin.Forms.CollectionView) has a name specified with the `x:Name` attribute. This enables the code-behind file to access the object using the assigned name.

1. In **Solution Pad**, in the **WebServiceTutorial** project, expand **MainPage.xaml** and double-click **MainPage.xaml.cs** to open it. Then, in **MainPage.xaml.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Xamarin.Forms;

    namespace WebServiceTutorial
    {
        public partial class MainPage : ContentPage
        {
            RestService _restService;

            public MainPage()
            {
                InitializeComponent();
                _restService = new RestService();
            }

            async void OnButtonClicked(object sender, EventArgs e)
            {
                List<Repository> repositories = await _restService.GetRepositoriesAsync(Constants.GitHubReposEndpoint);
                collectionView.ItemsSource = repositories;
            }
        }
    }
    ```

    The `OnButtonClicked` method, which is executed when the [`Button`](xref:Xamarin.Forms.Button) is tapped, invokes the `RestService.GetRepositoriesAsync` method to retrieve .NET repository data from the GitHub web API. The `GetRepositoriesAsync` method requires a `string` argument representing the URI of the web API being invoked, and this is returned by the `Constants.GitHubReposEndpoint` field.

    After retrieving the requested data, the [`CollectionView.ItemsSource`](xref:Xamarin.Forms.ItemsView`1.ItemsSource) property is set to the retrieved data.

1. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator. Tap the [`Button`](xref:Xamarin.Forms.Button) to retrieve .NET repository data from GitHub:

    [![Screenshot of GitHub .NET repositories, on iOS and Android](../images/consume-web-service.png)](../images/consume-web-service-large.png#lightbox)

    In Visual Studio for Mac, stop the application.

    For more information about consuming REST-based web services in Xamarin.Forms, see [Consume a RESTful Web Service (guide)](~/xamarin-forms/data-cloud/web-services/rest.md).
