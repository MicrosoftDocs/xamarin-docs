# [Visual Studio](#tab/vswin)

1. In **MainPage.xaml**, modify the [`Grid`](xref:Xamarin.Forms.Grid) declaration to define columns and rows, and place content in the columns and rows:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <Label Text="Column 0, Row 0" />
        <Label Grid.Column="1"
               Text="Column 1, Row 0" />
        <Label Grid.Row="1"
               Text="Column 0, Row 1" />
        <Label Grid.Column="1"
               Grid.Row="1"
               Text="Column 1, Row 1" />
    </Grid>
    ```

    This code defines columns and rows for the [`Grid`](xref:Xamarin.Forms.Grid), and positions [`Label`](xref:Xamarin.Forms.Label) instances in specific columns and rows. The column and row data is stored in the [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) and [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) properties, which are collections of [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) and [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) objects, respectively. The width of each `ColumnDefinition` is set by the [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width) property, and the height of each `RowDefinition` is set by the [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height) property. Valid width and height values are:

    - [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto), which sizes the column or row to fit the content.
    - Proportional values, which size the columns and rows as a proportion of the remaining space. Proportional values are indicated by ending with a `*`.
    - Absolute values, which size the columns or rows with specific, fixed values.

    Therefore, in the code above, each column has a width of half the [`Grid`](xref:Xamarin.Forms.Grid), while each row has a height of 50 device-independent units.

    The position of each [`Label`](xref:Xamarin.Forms.Label) in the [`Grid`](xref:Xamarin.Forms.Grid) is specified with the [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) and [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) attached properties, using a zero-based index. Therefore, the first column is column 0, and the first row is row 0. The first `Label` lacks these attached properties, because any child views that don't set them will automatically be rendered in column 0, row 0.

    > [!NOTE]
    > The spacing between columns and rows in a [`Grid`](xref:Xamarin.Forms.Grid) can be set with the [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) and [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) properties. For more information, see [Spacing](~/xamarin-forms/user-interface/layouts/grid.md#space-between-rows-and-columns) in the [Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md) guide.

1. If the application is still running, save the changes to the file and the application user interface will automatically be updated in your simulator or emulator. Otherwise, in the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator:

    [![Screenshot of a Grid that has content laid out in columns and rows, on iOS and Android](../images/columns-rows.png "Grid with content in columns and rows")](../images/columns-rows-large.png#lightbox "Grid with content in columns and rows")

# [Visual Studio for Mac](#tab/vsmac)

1. In **MainPage.xaml**, modify the [`Grid`](xref:Xamarin.Forms.Grid) declaration to define columns and rows, and place content in the columns and rows:

    ```xaml
    <Grid Margin="20,35,20,20">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="0.5*" />
            <ColumnDefinition Width="0.5*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="50" />
            <RowDefinition Height="50" />
        </Grid.RowDefinitions>
        <Label Text="Column 0, Row 0" />
        <Label Grid.Column="1"
               Text="Column 1, Row 0" />
        <Label Grid.Row="1"
               Text="Column 0, Row 1" />
        <Label Grid.Column="1"
               Grid.Row="1"
               Text="Column 1, Row 1" />
    </Grid>
    ```

    This code defines columns and rows for the [`Grid`](xref:Xamarin.Forms.Grid), and positions [`Label`](xref:Xamarin.Forms.Label) instances in specific columns and rows. The column and row data is stored in the [`ColumnDefinitions`](xref:Xamarin.Forms.Grid.ColumnDefinitions) and [`RowDefinitions`](xref:Xamarin.Forms.Grid.RowDefinitions) properties, which are collections of [`ColumnDefinition`](xref:Xamarin.Forms.ColumnDefinition) and [`RowDefinition`](xref:Xamarin.Forms.RowDefinition) objects, respectively. The width of each `ColumnDefinition` is set by the [`ColumnDefinition.Width`](xref:Xamarin.Forms.ColumnDefinition.Width) property, and the height of each `RowDefinition` is set by the [`RowDefinition.Height`](xref:Xamarin.Forms.RowDefinition.Height) property. Valid width and height values are:

    - [`Auto`](xref:Xamarin.Forms.GridUnitType.Auto), which sizes the column or row to fit the content.
    - Proportional values, which size the columns and rows as a proportion of the remaining space. Proportional values are indicated by ending with a `*`.
    - Absolute values, which size the columns or rows with specific, fixed values.

    Therefore, in the code above, each column has a width of half the [`Grid`](xref:Xamarin.Forms.Grid), while each row has a height of 50 device-independent units.

    The position of each [`Label`](xref:Xamarin.Forms.Label) in the [`Grid`](xref:Xamarin.Forms.Grid) is specified with the [`Grid.Column`](xref:Xamarin.Forms.Grid.ColumnProperty) and [`Grid.Row`](xref:Xamarin.Forms.Grid.RowProperty) attached properties, using a zero-based index. Therefore, the first column is column 0, and the first row is row 0. The first `Label` lacks these attached properties, because any child views that don't set them will automatically be rendered in column 0, row 0.

    > [!NOTE]
    > The spacing between columns and rows in a [`Grid`](xref:Xamarin.Forms.Grid) can be set with the [`ColumnSpacing`](xref:Xamarin.Forms.Grid.ColumnSpacing) and [`RowSpacing`](xref:Xamarin.Forms.Grid.RowSpacing) properties. For more information, see [Spacing](~/xamarin-forms/user-interface/layouts/grid.md#space-between-rows-and-columns) in the [Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md) guide.

1. If the application is still running, save the changes to the file and the application user interface will automatically be updated in your simulator or emulator. Otherwise, in the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator:

    [![Screenshot of a Grid that has content laid out in columns and rows, on iOS and Android](../images/columns-rows.png "Grid with content in columns and rows")](../images/columns-rows-large.png#lightbox "Grid with content in columns and rows")
