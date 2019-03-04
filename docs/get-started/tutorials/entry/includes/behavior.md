# [Visual Studio](#tab/vswin)

1. In **MainPage.xaml**, modify the [`Entry`](xref:Xamarin.Forms.Entry) declaration to customize its behavior:

    ```xaml
    <Entry Placeholder="Enter password"
           MaxLength="15"
           IsSpellCheckEnabled="false"
           IsTextPredictionEnabled="false"
           IsPassword="true" />
    ```

    This code sets properties that customize the behavior of the [`Entry`](xref:Xamarin.Forms.Entry). The [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) property limits the input length that's permitted for the `Entry`, and the [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) property is set to `false` to disable spell checking. Similarly, the [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) property is set to `false` to disable text prediction and automatic text prediction. In addition, the [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) property ensures that entered characters are masked with a password character (a black circle).

    > [!NOTE]
    > For some text entry scenarios, such as entering a password, spell checking and text prediction provides a negative experience and should therefore be disabled.

1. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator. Enter text into the [`Entry`](xref:Xamarin.Forms.Entry) and observe that each character is replaced with a password mask character, and that the maximum number of characters that can be entered is 15:

    [![Screenshot of an Entry with text masked by password characters, on iOS and Android](../images/customize-behavior.png "Entry with masked password characters")](../images/customize-behavior-large.png#lightbox "Entry with masked password characters")

    For more information about customizing [`Entry`](xref:Xamarin.Forms.Entry) behavior, see the [Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/entry.md) guide.

# [Visual Studio for Mac](#tab/vsmac)

1. In **MainPage.xaml**, modify the [`Entry`](xref:Xamarin.Forms.Entry) declaration to customize its behavior:

    ```xaml
    <Entry Placeholder="Enter password"
           MaxLength="15"
           IsSpellCheckEnabled="false"
           IsTextPredictionEnabled="false"
           IsPassword="true" />
    ```

    This code sets properties that customize the behavior of the [`Entry`](xref:Xamarin.Forms.Entry). The [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) property limits the input length that's permitted for the `Entry`, and the [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) property is set to `false` to disable spell checking. Similarly, the [`IsTextPredictionEnabled`](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) property is set to `false` to disable text prediction and automatic text prediction. In addition, the [`IsPassword`](xref:Xamarin.Forms.Entry.IsPassword) property ensures that entered characters are masked with a password character (a black circle).

    > [!NOTE]
    > For some text entry scenarios, such as entering a password, spell checking and text prediction provides a negative experience and should therefore be disabled.

1. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator. Enter text into the [`Entry`](xref:Xamarin.Forms.Entry) and observe that each character is replaced with a password mask character, and that the maximum number of characters that can be entered is 15:

    [![Screenshot of an Entry with text masked by password characters, on iOS and Android](../images/customize-behavior.png "Entry with masked password characters")](../images/customize-behavior-large.png#lightbox "Entry with masked password characters")

    For more information about customizing [`Entry`](xref:Xamarin.Forms.Entry) behavior, see the [Xamarin.Forms Entry](~/xamarin-forms/user-interface/text/entry.md) guide.
