In this exercise you will create a user interface to consume `RestService` class, which in turn retrieves data from the [OpenWeatherMap](https://openweathermap.org/) web API.

# [Visual Studio](#tab/vswin)

1. In **Solution Explorer**, in the **WebServiceTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.4*" />
                <ColumnDefinition Width="0.6*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
            </Grid.RowDefinitions>
            <Entry x:Name="cityEntry"
                   Grid.ColumnSpan="2"
                   Text="Seattle" />
            <Button Grid.ColumnSpan="2"
                    Grid.Row="1"
                    Text="Get Weather"
                    Clicked="OnButtonClicked" />                
            <Label Grid.Row="2"
                   Text="Location:" />
            <Label Grid.Row="2"
                   Grid.Column="1"
                   Text="{Binding Title}" />            
            <Label Grid.Row="3"
                   Text="Temperature:" />
            <Label Grid.Row="3"
                   Grid.Column="1"
                   Text="{Binding Main.Temperature}" />            
            <Label Grid.Row="4"
                   Text="Wind Speed:" />
            <Label Grid.Row="4"
                   Grid.Column="1"
                   Text="{Binding Wind.Speed}" />            
            <Label Grid.Row="5"
                   Text="Humidity:" />
            <Label Grid.Row="5"
                   Grid.Column="1"
                   Text="{Binding Main.Humidity}" />            
            <Label Grid.Row="6"
                   Text="Visibility:" />
            <Label Grid.Row="6"
                   Grid.Column="1"
                   Text="{Binding Weather[0].Visibility}" />
        </Grid>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of an [`Entry`](xref:Xamarin.Forms.Entry), a [`Button`](xref:Xamarin.Forms.Button), and a series of [`Label`](xref:Xamarin.Forms.Label) instances in a [`Grid`](xref:Xamarin.Forms.Grid). The `Entry` is pre-populated with "Seattle" by setting its [`Text`](xref:Xamarin.Forms.InputView.Text) property. The `Button` sets its [`Clicked`](xref:Xamarin.Forms.Button.Clicked) event to an event handler named `OnButtonClicked` that will be created in the next step. Half of the `Label` instances display static text, with the remaining instances data binding to `WeatherData` properties. At runtime, the `Label` instances that use data binding will look at their respective [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) properties for the `WeatherData` object to use in their binding expressions. For more information about data binding, see [Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    In addition, the [`Entry`](xref:Xamarin.Forms.Entry) has a name specified with the `x:Name` attribute. This enables the code-behind file to access the object using the assigned name.

1. In **Solution Explorer**, in the **WebServiceTutorial** project, expand **MainPage.xaml** and double-click **MainPage.xaml.cs** to open it. Then, in **MainPage.xaml.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
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
                if (!string.IsNullOrWhiteSpace(cityEntry.Text))
                {
                    WeatherData weatherData = await _restService.GetWeatherDataAsync(GenerateRequestUri(Constants.OpenWeatherMapEndpoint));
                    BindingContext = weatherData;
                }
            }

            string GenerateRequestUri(string endpoint)
            {
                string requestUri = endpoint;
                requestUri += $"?q={cityEntry.Text}";
                requestUri += "&units=imperial"; // or units=metric
                requestUri += $"&APPID={Constants.OpenWeatherMapAPIKey}";
                return requestUri;
            }
        }
    }
    ```

    The `OnButtonClicked` method, which is executed when the [`Button`](xref:Xamarin.Forms.Button) is tapped, invokes the `RestService.GetWeatherDataAsync` method to retrieve weather data for the city whose name has been entered in the [`Entry`](xref:Xamarin.Forms.Entry). The `GetWeatherDataAsync` method requires a `string` argument representing the URI of the web API being invoked, and this is produced by the `GenerateRequestUri` method. This method takes the endpoint address stored in the `OpenWeatherMapEndpoint` constant, and adds query parameters to it that specify:

    - The city that weather data is requested for.
    - The units to return weather data in.
    - Your personal API key.

    After retrieving the requested weather data, the [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) of the page is set to the `WeatherData` object. For more information about the `BindingContext` property, see [Bindings with a Binding Context](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context) in the [Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md) guide.

    > [!IMPORTANT]
    > The [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) property is inherited through the visual tree. Therefore, because it's been set on the [`ContentPage`](xref:Xamarin.Forms.ContentPage) object, child objects of the `ContentPage` inherit its value, including the [`Label`](xref:Xamarin.Forms.Label) instances.

1. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator. Tap the [`Button`](xref:Xamarin.Forms.Button) to retrieve current weather data for Seattle:

    [![Screenshot of weather data for Seattle, on iOS and Android](../images/consume-web-service.png "Seattle weather data")](../images/consume-web-service-large.png#lightbox "Seattle weather data")

    > [!IMPORTANT]
    > Your personal OpenWeatherMap API key must be set as the value of the `OpenWeatherMapAPIKey` constant in the `Constants` class.

# [Visual Studio for Mac](#tab/vsmac)

1. In **Solution Pad**, in the **WebServiceTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="WebServiceTutorial.MainPage">
        <Grid Margin="20,35,20,20">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.4*" />
                <ColumnDefinition Width="0.6*" />
            </Grid.ColumnDefinitions>
            <Grid.RowDefinitions>
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
                <RowDefinition Height="40" />
            </Grid.RowDefinitions>
            <Entry x:Name="cityEntry"
                   Grid.ColumnSpan="2"
                   Text="Seattle" />
            <Button Grid.ColumnSpan="2"
                    Grid.Row="1"
                    Text="Get Weather"
                    Clicked="OnButtonClicked" />                
            <Label Grid.Row="2"
                   Text="Location:" />
            <Label Grid.Row="2"
                   Grid.Column="1"
                   Text="{Binding Title}" />            
            <Label Grid.Row="3"
                   Text="Temperature:" />
            <Label Grid.Row="3"
                   Grid.Column="1"
                   Text="{Binding Main.Temperature}" />            
            <Label Grid.Row="4"
                   Text="Wind Speed:" />
            <Label Grid.Row="4"
                   Grid.Column="1"
                   Text="{Binding Wind.Speed}" />            
            <Label Grid.Row="5"
                   Text="Humidity:" />
            <Label Grid.Row="5"
                   Grid.Column="1"
                   Text="{Binding Main.Humidity}" />            
            <Label Grid.Row="6"
                   Text="Visibility:" />
            <Label Grid.Row="6"
                   Grid.Column="1"
                   Text="{Binding Weather[0].Visibility}" />
        </Grid>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of an [`Entry`](xref:Xamarin.Forms.Entry), a [`Button`](xref:Xamarin.Forms.Button), and a series of [`Label`](xref:Xamarin.Forms.Label) instances in a [`Grid`](xref:Xamarin.Forms.Grid). The `Entry` is pre-populated with "Seattle" by setting its [`Text`](xref:Xamarin.Forms.InputView.Text) property. The `Button` sets its [`Clicked`](xref:Xamarin.Forms.Button.Clicked) event to an event handler named `OnButtonClicked` that will be created in the next step. Half of the `Label` instances display static text, with the remaining instances data binding to `WeatherData` properties. At runtime, the `Label` instances that use data binding will look at their respective [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) properties for the `WeatherData` object to use in their binding expressions. For more information about data binding, see [Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md).

    In addition, the [`Entry`](xref:Xamarin.Forms.Entry) has a name specified with the `x:Name` attribute. This enables the code-behind file to access the object using the assigned name.

    For more information about consuming REST-based web services in Xamarin.Forms, see [Consume a RESTful Web Service (guide)](~/xamarin-forms/data-cloud/web-services/rest.md).

1. In **Solution Pad**, in the **WebServiceTutorial** project, expand **MainPage.xaml** and double-click **MainPage.xaml.cs** to open it. Then, in **MainPage.xaml.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
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
                if (!string.IsNullOrWhiteSpace(cityEntry.Text))
                {
                    WeatherData weatherData = await _restService.GetWeatherDataAsync(GenerateRequestUri(Constants.OpenWeatherMapEndpoint));
                    BindingContext = weatherData;
                }
            }

            string GenerateRequestUri(string endpoint)
            {
                string requestUri = endpoint;
                requestUri += $"?q={cityEntry.Text}";
                requestUri += "&units=imperial"; // or units=metric
                requestUri += $"&APPID={Constants.OpenWeatherMapAPIKey}";
                return requestUri;
            }
        }
    }
    ```

    The `OnButtonClicked` method, which is executed when the [`Button`](xref:Xamarin.Forms.Button) is tapped, invokes the `RestService.GetWeatherDataAsync` method to retrieve weather data for the city whose name has been entered in the [`Entry`](xref:Xamarin.Forms.Entry). The `GetWeatherDataAsync` method requires a `string` argument representing the URI of the web API being invoked, and this is produced by the `GenerateRequestUri` method. This method takes the endpoint address stored in the `OpenWeatherMapEndpoint` constant, and adds query parameters to it that specify:

    - The city that weather data is requested for.
    - The units to return weather data in.
    - Your personal API key.

    After retrieving the requested weather data, the [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) of the page is set to the `WeatherData` object. For more information about the `BindingContext` property, see [Bindings with a Binding Context](~/xamarin-forms/app-fundamentals/data-binding/basic-bindings.md#bindings-with-a-binding-context) in the [Xamarin.Forms Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md) guide.

    > [!IMPORTANT]
    > The [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) property is inherited through the visual tree. Therefore, because it's been set on the [`ContentPage`](xref:Xamarin.Forms.ContentPage) object, child objects of the `ContentPage` inherit its value, including the [`Label`](xref:Xamarin.Forms.Label) instances.

1. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator. Tap the [`Button`](xref:Xamarin.Forms.Button) to retrieve current weather data for Seattle:

    [![Screenshot of weather data for Seattle, on iOS and Android](../images/consume-web-service.png "Seattle weather data")](../images/consume-web-service-large.png#lightbox "Seattle weather data")

    > [!IMPORTANT]
    > Your personal OpenWeatherMap API key must be set as the value of the `OpenWeatherMapAPIKey` constant in the `Constants` class.

    For more information about consuming REST-based web services in Xamarin.Forms, see [Consume a RESTful Web Service (guide)](~/xamarin-forms/data-cloud/web-services/rest.md).
