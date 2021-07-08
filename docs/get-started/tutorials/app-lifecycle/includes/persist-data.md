The [`Application`](xref:Xamarin.Forms.Application) subclass has a static [`Properties`](xref:Xamarin.Forms.Application.Properties) dictionary that can be used to store data across lifecycle state changes. This dictionary uses a `string` key and stores an `object` value. The dictionary is saved to the device automatically, and is repopulated when the application is restarted.

> [!IMPORTANT]
> The [`Properties`](xref:Xamarin.Forms.Application.Properties) dictionary can only serialize primitive types for storage.

In this exercise, you'll modify the application to persist the text from an [`Entry`](xref:Xamarin.Forms.Entry) upon backgrounding, and restore the text to the `Entry` when the application is restarted.

# [Visual Studio](#tab/vswin)

1. In **Solution Explorer**, in the **AppLifecycleTutorial** project, expand **App.xaml** and double-click **App.xaml.cs** to open it. Then, in **App.xaml.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    This code defines a `DisplayText` property, and a `displayText` constant. When the application is backgrounded, or terminated, the `OnSleep` method override adds the `DisplayText` property value to the [`Properties`](xref:Xamarin.Forms.Application.Properties) dictionary, against a key of `displayText`. Then when the application starts, provided that the `Properties` dictionary contains the `displayText` key, the value for the key is restored to the `DisplayText` property.

    > [!IMPORTANT]
    > Always check the [`Properties`](xref:Xamarin.Forms.Application.Properties) dictionary for the presence of a key before accessing it, to prevent unexpected errors.

    It's not necessary to restore data from the [`Properties`](xref:Xamarin.Forms.Application.Properties) dictionary in the `OnResume` method overload. This is because when an application is backgrounded, it and its state is still in memory.

1. In **Solution Explorer**, in the **AppLifecycleTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of an [`Entry`](xref:Xamarin.Forms.Entry) in a [`StackLayout`](xref:Xamarin.Forms.StackLayout). The [`Entry.Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) property specifies the placeholder text that's shown when the `Entry` is first displayed, and an event handler named `OnEntryCompleted` is registered with the [`Completed`](xref:Xamarin.Forms.Entry.Completed) event. In addition, the `Entry` has a name specified with the `x:Name` attribute. This enables the code-behind file to access the `Entry` object using the name assigned to it.

1. In **Solution Explorer**, in the **AppLifecycleTutorial** project, expand **MainPage.xaml** and double-click **MainPage.xaml.cs** to open it. Then, in **MainPage.xaml.cs**, add an override for the `OnAppearing` method, and the `OnEntryCompleted` event handler to the class:

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    The `OnAppearing` method retrieves the value of the `App.DisplayText` property and sets it as the [`Text`](xref:Xamarin.Forms.InputView.Text) property value of the [`Entry`](xref:Xamarin.Forms.Entry).

    > [!NOTE]
    > The `OnAppearing` method override is executed after the [`ContentPage`](xref:Xamarin.Forms.ContentPage) is laid out, but just before it becomes visible. Therefore, this is a good place to set the content of Xamarin.Forms views.

    When text is finalized in the [`Entry`](xref:Xamarin.Forms.Entry), with the return key, the `OnEntryCompleted` method executes and the `Entry` text is stored in the `App.DisplayText` property.

1. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator.

    Enter some text into the [`Entry`](xref:Xamarin.Forms.Entry) and hit the return key. Then, background the application by tapping the Home button to invoke the `OnSleep` method.

    In Visual Studio, stop the application and relaunch it again, and the text that was previously entered into the [`Entry`](xref:Xamarin.Forms.Entry) will be restored:

    [![Screenshot of an Entry whose Text property is persisted across lifecycle state changes, on iOS and Android](../images/persist-data.png "Entry whose Text property persists across lifecycle state changes")](../images/persist-data-large.png#lightbox "Entry whose Text property persists across lifecycle state changes")

    In Visual Studio, stop the application.

    For more information about persisting data to the properties dictionary, see [Properties Dictionary](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary) in the [Xamarin.Forms App Class](~/xamarin-forms/app-fundamentals/application-class.md) guide.

# [Visual Studio for Mac](#tab/vsmac)

1. In **Solution Pad**, in the **AppLifecycleTutorial** project, expand **App.xaml** and double-click **App.xaml.cs** to open it. Then, in **App.xaml.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using Xamarin.Forms;

    namespace AppLifecycleTutorial
    {
        public partial class App : Application
        {
            const string displayText = "displayText";

            public string DisplayText { get; set; }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                Console.WriteLine("OnStart");

                if (Properties.ContainsKey(displayText))
                {
                    DisplayText = (string)Properties[displayText];
                }
            }

            protected override void OnSleep()
            {
                Console.WriteLine("OnSleep");
                Properties[displayText] = DisplayText;
            }

            protected override void OnResume()
            {
                Console.WriteLine("OnResume");
            }
        }
    }
    ```

    This code defines a `DisplayText` property, and a `displayText` constant. When the application is backgrounded, or terminated, the `OnSleep` method override adds the `DisplayText` property value to the [`Properties`](xref:Xamarin.Forms.Application.Properties) dictionary, against a key of `displayText`. Then when the application starts, provided that the `Properties` dictionary contains the `displayText` key, the value for the key is restored to the `DisplayText` property.

    > [!IMPORTANT]
    > Always check the [`Properties`](xref:Xamarin.Forms.Application.Properties) dictionary for the presence of a key before accessing it, to prevent unexpected errors.

    It's not necessary to restore data from the [`Properties`](xref:Xamarin.Forms.Application.Properties) dictionary in the `OnResume` method overload. This is because when an application is backgrounded, it and its state is still in memory.

1. In **Solution Pad**, in the **AppLifecycleTutorial** project, double-click **MainPage.xaml** to open it. Then, in **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="AppLifecycleTutorial.MainPage">
        <StackLayout Margin="20,35,20,20">
            <Entry x:Name="entry"
                   Placeholder="Enter text here"
                   Completed="OnEntryCompleted" />
        </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of an [`Entry`](xref:Xamarin.Forms.Entry) in a [`StackLayout`](xref:Xamarin.Forms.StackLayout). The [`Entry.Placeholder`](xref:Xamarin.Forms.InputView.Placeholder) property specifies the placeholder text that's shown when the `Entry` is first displayed, and an event handler named `OnEntryCompleted` is registered with the [`Completed`](xref:Xamarin.Forms.Entry.Completed) event. In addition, the `Entry` has a name specified with the `x:Name` attribute. This enables the code-behind file to access the `Entry` object using the name assigned to it.

1. In **Solution Pad**, in the **AppLifecycleTutorial** project, expand **MainPage.xaml** and double-click **MainPage.xaml.cs** to open it. Then, in **MainPage.xaml.cs**, add an override for the `OnAppearing` method, and the `OnEntryCompleted` event handler to the class:

    ```csharp
    protected override void OnAppearing()
    {
        base.OnAppearing();

        entry.Text = (Application.Current as App).DisplayText;
    }

    void OnEntryCompleted(object sender, EventArgs e)
    {
        (Application.Current as App).DisplayText = entry.Text;
    }
    ```

    The `OnAppearing` method retrieves the value of the `App.DisplayText` property and sets it as the [`Text`](xref:Xamarin.Forms.InputView.Text) property value of the [`Entry`](xref:Xamarin.Forms.Entry).

    > [!NOTE]
    > The `OnAppearing` method override is executed after the [`ContentPage`](xref:Xamarin.Forms.ContentPage) is laid out, but just before it becomes visible. Therefore, this is a good place to set the content of Xamarin.Forms views.

    When text is finalized in the [`Entry`](xref:Xamarin.Forms.Entry), with the return key, the `OnEntryCompleted` method executes and the `Entry` text is stored in the `App.DisplayText` property.

1. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator.

    Enter some text into the [`Entry`](xref:Xamarin.Forms.Entry) and hit the return key. Then, background the application by tapping the Home button to invoke the `OnSleep` method.

    In Visual Studio for Mac, stop the application and relaunch it again, and the text that was previously entered into the [`Entry`](xref:Xamarin.Forms.Entry) will be restored:

    [![Screenshot of an Entry whose Text property is persisted across lifecycle state changes, on iOS and Android](../images/persist-data.png "Entry whose Text property persists across lifecycle state changes")](../images/persist-data-large.png#lightbox "Entry whose Text property persists across lifecycle state changes")

    In Visual Studio for Mac, stop the application.

    For more information about persisting data to the properties dictionary, see [Properties Dictionary](~/xamarin-forms/app-fundamentals/application-class.md#properties-dictionary) in the [Xamarin.Forms App Class](~/xamarin-forms/app-fundamentals/application-class.md) guide.
