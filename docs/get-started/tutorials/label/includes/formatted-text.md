# [Visual Studio](#tab/vswin)

1. In **MainPage.xaml**, modify the [`Label`](xref:Xamarin.Forms.Label) declaration to present text that uses multiple formats, in a single `Label`.

    ```xaml
    <Label TextColor="Gray"
           FontSize="Medium">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="This sentence contains " />
                <Span Text="words that are emphasized, "
                      FontAttributes="Italic" />
                <Span Text="and underlined."
                      TextDecorations="Underline" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
    ```

    This code displays text, in a single [`Label`](xref:Xamarin.Forms.Label), that uses multiple formats. The text in the first [`Span`](xref:Xamarin.Forms.Span) is displayed using the formatting set on the `Label`, while the text in the second and third `Span` instances is displayed using the formatting set on the `Label` and the additional formatting set on each `Span`.

    > [!NOTE]
    > The [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) property is of type [`FormattedString`](xref:Xamarin.Forms.FormattedString), which comprises one or more [`Span`](xref:Xamarin.Forms.Span) instances.

1. If the application is still running, save the changes to the file and the application user interface will automatically be updated in your simulator or emulator. Otherwise, in the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator. Observe that the [`Label`](xref:Xamarin.Forms.Label) appearance has changed:

    [![Screenshot of a Label displaying formatted text, on iOS and Android](../images/label-formatted-text.png "Label with formatted text")](../images/label-formatted-text-large.png#lightbox "Label with formatted text")

    In Visual Studio, stop the application.

    For more information about setting [`Span`](xref:Xamarin.Forms.Span) appearance, see [Formatted text](~/xamarin-forms/user-interface/text/label.md#formatted-text) in the [Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md) guide.

# [Visual Studio for Mac](#tab/vsmac)

1. In **MainPage.xaml**, modify the [`Label`](xref:Xamarin.Forms.Label) declaration to present text that uses multiple formats, in a single `Label`.

    ```xaml
    <Label TextColor="Gray"
           FontSize="Medium">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="This sentence contains " />
                <Span Text="words that are emphasized, "
                      FontAttributes="Italic" />
                <Span Text="and underlined."
                      TextDecorations="Underline" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
    ```

    This code displays text, in a single [`Label`](xref:Xamarin.Forms.Label), that uses multiple formats. The text in the first [`Span`](xref:Xamarin.Forms.Span) is displayed using the formatting set on the `Label`, while the text in the second and third `Span` instances is displayed using the formatting set on the `Label` and the additional formatting set on each `Span`.

    > [!NOTE]
    > The [`FormattedText`](xref:Xamarin.Forms.Label.FormattedText) property is of type [`FormattedString`](xref:Xamarin.Forms.FormattedString), which comprises one or more [`Span`](xref:Xamarin.Forms.Span) instances.

1. If the application is still running, save the changes to the file and the application user interface will automatically be updated in your simulator or emulator. Otherwise, in the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator. Observe that the [`Label`](xref:Xamarin.Forms.Label) appearance has changed:

    [![Screenshot of a Label displaying formatted text, on iOS and Android](../images/label-formatted-text.png "Label with formatted text")](../images/label-formatted-text-large.png#lightbox "Label with formatted text")

    In Visual Studio for Mac, stop the application.

    For more information about setting [`Span`](xref:Xamarin.Forms.Span) appearance, see [Formatted text](~/xamarin-forms/user-interface/text/label.md#formatted-text) in the [Xamarin.Forms Label](~/xamarin-forms/user-interface/text/label.md) guide.
