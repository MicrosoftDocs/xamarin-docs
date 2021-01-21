Previously, the [`CollectionView`](xref:Xamarin.Forms.CollectionView) was populated with data using data binding. However, despite data binding to a collection, where each object in the collection defined multiple items of data, only a single item of data was displayed per object (the `Name` property of the `Monkey` object).

In this exercise, you will modify the **CollectionViewTutorial** project so that the [`CollectionView`](xref:Xamarin.Forms.CollectionView) displays multiple items of data in each row.

# [Visual Studio](#tab/vswin)

1. In **MainPage.xaml**, modify the [`CollectionView`](xref:Xamarin.Forms.CollectionView) declaration to customize the appearance of each item of data:

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}"
                    SelectionMode="Single"
                    SelectionChanged="OnSelectionChanged">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10"
                      RowDefinitions="Auto, *"
                      ColumnDefinitions="Auto, *">
                    <Image Grid.RowSpan="2"
                           Source="{Binding ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Name}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Location}"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
    ```

    This code sets the [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), which defines the appearance of each item in the [`CollectionView`](xref:Xamarin.Forms.CollectionView). The child of the `DataTemplate` is a [`Grid`](xref:Xamarin.Forms.Grid), which contains an [`Image`](xref:Xamarin.Forms.Image) object and two [`Label`](xref:Xamarin.Forms.Label) objects. The `Image` object data binds its [`Source`](xref:Xamarin.Forms.Image.Source) property to the `ImageUrl` property of each `Monkey` object, while the `Label` objects bind their [`Text`](xref:Xamarin.Forms.Label.Text) properties to the `Name` and `Location` properties of each `Monkey` object.

    For more information about [`CollectionView`](xref:Xamarin.Forms.CollectionView) item appearance, see [Define item appearance](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance). For more information about data templates, see [Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

1. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator:

    [![Screenshot of a CollectionView whose items are templated with a data template](../images/customize-item-appearance.png "CollectionView displaying templated data")](../images/customize-item-appearance-large.png#lightbox "CollectionView displaying templated data")

    In Visual Studio, stop the application.

# [Visual Studio for Mac](#tab/vsmac)

1. In **MainPage.xaml**, modify the [`CollectionView`](xref:Xamarin.Forms.CollectionView) declaration to customize the appearance of each item of data:

    ```xaml
    <CollectionView ItemsSource="{Binding Monkeys}"
                    SelectionMode="Single"
                    SelectionChanged="OnSelectionChanged">
        <CollectionView.ItemTemplate>
            <DataTemplate>
                <Grid Padding="10"
                      RowDefinitions="Auto, *"
                      ColumnDefinitions="Auto, *">
                    <Image Grid.RowSpan="2"
                           Source="{Binding ImageUrl}"
                           Aspect="AspectFill"
                           HeightRequest="60"
                           WidthRequest="60" />
                    <Label Grid.Column="1"
                           Text="{Binding Name}"
                           FontAttributes="Bold" />
                    <Label Grid.Row="1"
                           Grid.Column="1"
                           Text="{Binding Location}"
                           VerticalOptions="End" />
                </Grid>
            </DataTemplate>
        </CollectionView.ItemTemplate>
    </CollectionView>
    ```

    This code sets the [`CollectionView.ItemTemplate`](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), which defines the appearance of each item in the [`CollectionView`](xref:Xamarin.Forms.CollectionView). The child of the `DataTemplate` is a [`Grid`](xref:Xamarin.Forms.Grid), which contains an [`Image`](xref:Xamarin.Forms.Image) object and two [`Label`](xref:Xamarin.Forms.Label) objects. The `Image` object data binds its [`Source`](xref:Xamarin.Forms.Image.Source) property to the `ImageUrl` property of each `Monkey` object, while the `Label` objects bind their [`Text`](xref:Xamarin.Forms.Label.Text) properties to the `Name` and `Location` properties of each `Monkey` object.

    For more information about [`CollectionView`](xref:Xamarin.Forms.CollectionView) item appearance, see [Define item appearance](~/xamarin-forms/user-interface/collectionview/populate-data.md#define-item-appearance). For more information about data templates, see [Xamarin.Forms Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md).

1. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator:

    [![Screenshot of a CollectionView whose items are templated with a data template](../images/customize-item-appearance.png "CollectionView displaying templated data")](../images/customize-item-appearance-large.png#lightbox "CollectionView displaying templated data")

    In Visual Studio for Mac, stop the application.
