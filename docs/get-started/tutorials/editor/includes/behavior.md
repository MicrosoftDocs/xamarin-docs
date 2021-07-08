# [Visual Studio](#tab/vswin)

1. In **MainPage.xaml**, modify the [`Editor`](xref:Xamarin.Forms.Editor) declaration to customize its behavior:

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            AutoSize="TextChanges"
            MaxLength="200"
            IsSpellCheckEnabled="false"
            IsTextPredictionEnabled="false" />
    ```

    This code sets properties that customize the behavior of the [`Editor`](xref:Xamarin.Forms.Editor). The [`AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) property is set to [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges), which indicates that the height of the `Editor` will increase when its filled with text, and decrease as text is removed. The [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) property limits the input length that's permitted for the `Editor`. In addition, the [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) property is set to `false` to disable spell checking, while the `IsTextPredictionEnabled` property is set to `false` to disable text prediction and automatic text prediction.

1. If the application is still running, save the changes to the file and the application user interface will automatically be updated in your simulator or emulator. Otherwise, in the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator. Enter text into the [`Editor`](xref:Xamarin.Forms.Entry) and observe that the height of the `Editor` increases as it fills with text, and decreases as text is removed, and that the maximum number of characters that can be entered is 200:

    [![Screenshot of an auto-sizing Editor, on iOS and Android](../images/customize-behavior.png "Auto-sizing Editor")](../images/customize-behavior-large.png#lightbox "Auto-sizing Editor")

    In Visual Studio, stop the application.

    For more information about customizing [`Editor`](xref:Xamarin.Forms.Editor) behavior, see the [Xamarin.Forms Editor](~/xamarin-forms/user-interface/text/editor.md) guide.

# [Visual Studio for Mac](#tab/vsmac)

1. In **MainPage.xaml**, modify the [`Editor`](xref:Xamarin.Forms.Editor) declaration to customize its behavior:

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            AutoSize="TextChanges"
            MaxLength="200"
            IsSpellCheckEnabled="false"
            IsTextPredictionEnabled="false" />
    ```

    This code sets properties that customize the behavior of the [`Editor`](xref:Xamarin.Forms.Editor). The [`AutoSize`](xref:Xamarin.Forms.Editor.AutoSize) property is set to [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges), which indicates that the height of the `Editor` will increase when its filled with text, and decrease as text is removed. The [`MaxLength`](xref:Xamarin.Forms.InputView.MaxLength) property limits the input length that's permitted for the `Editor`. In addition, the [`IsSpellCheckEnabled`](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) property is set to `false` to disable spell checking, while the `IsTextPredictionEnabled` property is set to `false` to disable text prediction and automatic text prediction.

1. If the application is still running, save the changes to the file and the application user interface will automatically be updated in your simulator or emulator. Otherwise, in the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator. Enter text into the [`Editor`](xref:Xamarin.Forms.Entry) and observe that the height of the `Editor` increases as it fills with text, and decreases as text is removed, and that the maximum number of characters that can be entered is 200:

    [![Screenshot of an auto-sizing Editor, on iOS and Android](../images/customize-behavior.png "Auto-sizing Editor")](../images/customize-behavior-large.png#lightbox "Auto-sizing Editor")

    In Visual Studio for Mac, stop the application.

    For more information about customizing [`Editor`](xref:Xamarin.Forms.Editor) behavior, see the [Xamarin.Forms Editor](~/xamarin-forms/user-interface/text/editor.md) guide.
