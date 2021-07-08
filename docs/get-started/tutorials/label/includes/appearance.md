# [Visual Studio](#tab/vswin)

1. In **MainPage.xaml**, modify the [`Label`](xref:Xamarin.Forms.Label) declaration to change its visual appearance:

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    This code sets properties that change the visual appearance of the [`Label`](xref:Xamarin.Forms.Label). The [`TextColor`](xref:Xamarin.Forms.Label.TextColor) property sets the color of the `Label` text. The [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) property sets the font for the label to italic, and the [`FontSize`](xref:Xamarin.Forms.Label.FontSize) property sets the font size. In addition, an underline text decoration is applied to the `Label` by setting its [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations) property, and it's horizontally centered by setting the [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) property to [`Center`](xref:Xamarin.Forms.LayoutOptions.Center).

1. If the application is still running, save the changes to the file and the application user interface will automatically be updated in your simulator or emulator. Otherwise, in the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator. Observe that the [`Label`](xref:Xamarin.Forms.Label) appearance has changed:

    [![Screenshot of a Label with a changed visual appearance, on iOS and Android](../images/change-label-appearance.png "Label with changed appearance")](../images/change-label-appearance-large.png#lightbox "Label with changed appearance")

    For more information about setting [`Label`](xref:Xamarin.Forms.Label) appearance, see the [Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md) guide.

# [Visual Studio for Mac](#tab/vsmac)

1. In **MainPage.xaml**, modify the [`Label`](xref:Xamarin.Forms.Label) declaration to change its visual appearance:

    ```xaml
    <Label Text="Welcome to Xamarin.Forms!"
           TextColor="Blue"
           FontAttributes="Italic"
           FontSize="22"
           TextDecorations="Underline"
           HorizontalOptions="Center" />
    ```

    This code sets properties that change the visual appearance of the [`Label`](xref:Xamarin.Forms.Label). The [`TextColor`](xref:Xamarin.Forms.Label.TextColor) property sets the color of the `Label` text. The [`FontAttributes`](xref:Xamarin.Forms.Label.FontAttributes) property sets the font for the label to italic, and the [`FontSize`](xref:Xamarin.Forms.Label.FontSize) property sets the font size. In addition, an underline text decoration is applied to the `Label` by setting its [`TextDecorations`](xref:Xamarin.Forms.Label.TextDecorations) property, and it's horizontally centered by setting the [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) property to [`Center`](xref:Xamarin.Forms.LayoutOptions.Center).

1. If the application is still running, save the changes to the file and the application user interface will automatically be updated in your simulator or emulator. Otherwise, in the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator. Observe that the [`Label`](xref:Xamarin.Forms.Label) appearance has changed:

    [![Screenshot of a Label with a changed visual appearance, on iOS and Android](../images/change-label-appearance.png "Label with changed appearance")](../images/change-label-appearance-large.png#lightbox "Label with changed appearance")

    For more information about setting [`Label`](xref:Xamarin.Forms.Label) appearance, see the [Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md) guide.
