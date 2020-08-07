# [Visual Studio](#tab/vswin)

1. In **MainPage.xaml**, modify the [`Editor`](xref:Xamarin.Forms.Editor) declaration so that it sets a handler for the [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) and [`Completed`](xref:Xamarin.Forms.Editor.Completed) events:

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            HeightRequest="200"
            TextChanged="OnEditorTextChanged"
            Completed="OnEditorCompleted" />
    ```

    This code sets the [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) event to an event handler named `OnEditorTextChanged`, and the [`Completed`](xref:Xamarin.Forms.Editor.Completed) event to an event handler named `OnEditorCompleted`. Both event handlers will be created in the next step.

1. In **Solution Explorer**, in the **EditorTutorial** project, expand **MainPage.xaml** and double-click **MainPage.xaml.cs** to open it. Then, in **MainPage.xaml.cs**, add the `OnEditorTextChanged` and `OnEditorCompleted` event handlers to the class:

    ```csharp
    void OnEditorTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEditorCompleted(object sender, EventArgs e)
    {
        string text = ((Editor)sender).Text;
    }
    ```

    When the text in the [`Editor`](xref:Xamarin.Forms.Editor) changes, the `OnEditorTextChanged` method executes. The `sender` argument is the `Editor` object responsible for firing the `TextChanged` event, and can be used to access the `Editor` object. The [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) argument provides the old and new text values, from before and after the text change.

    When the editing is completed, the `OnEditorCompleted` method executes. This is achieved by unfocussing the [`Editor`](xref:Xamarin.Forms.Editor), or additionally by pressing the "Done" button on iOS. The `sender` argument is the `Editor` object responsible for firing the `TextChanged` event, and can be used to access the `Editor` object.

    > [!IMPORTANT]
    > Any text entered into an [`Editor`](xref:Xamarin.Forms.Editor) is stored in the [`Text`](xref:Xamarin.Forms.InputView.Text) property.

1. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen remote iOS simulator or Android emulator:

    [![Screenshot of Editor containing text, on iOS and Android](../images/text-changes.png "Editor with text")](../images/text-changes-large.png#lightbox "Editor with text")

    Set breakpoints in the two event handlers, enter text into the [`Editor`](xref:Xamarin.Forms.Editor), and observe the [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) event firing. Unfocus the `Editor` to observe the [`Completed`](xref:Xamarin.Forms.Entry.Completed) event firing.

    For more information about [`Editor`](xref:Xamarin.Forms.Editor) events, see [Events and interactivity](~/xamarin-forms/user-interface/text/editor.md#events-and-interactivity) in the [Xamarin.Forms Editor](~/xamarin-forms/user-interface/text/editor.md) guide.

# [Visual Studio for Mac](#tab/vsmac)

1. In **MainPage.xaml**, modify the [`Editor`](xref:Xamarin.Forms.Editor) declaration so that it sets a handler for the [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) and [`Completed`](xref:Xamarin.Forms.Editor.Completed) events:

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            HeightRequest="200"
            TextChanged="OnEditorTextChanged"
            Completed="OnEditorCompleted" />
    ```

    This code sets the [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) event to an event handler named `OnEditorTextChanged`, and the [`Completed`](xref:Xamarin.Forms.Editor.Completed) event to an event handler named `OnEditorCompleted`. Both event handlers will be created in the next step.

1. In **Solution Pad**, in the **EditorTutorial** project, expand **MainPage.xaml** and double-click **MainPage.xaml.cs** to open it. Then, in **MainPage.xaml.cs**, add the `OnEditorTextChanged` and `OnEditorCompleted` event handlers to the class:

    ```csharp
    void OnEditorTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEditorCompleted(object sender, EventArgs e)
    {
        string text = ((Editor)sender).Text;
    }
    ```

    When the text in the [`Editor`](xref:Xamarin.Forms.Editor) changes, the `OnEditorTextChanged` method executes. The `sender` argument is the `Editor` object responsible for firing the `TextChanged` event, and can be used to access the `Editor` object. The [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) argument provides the old and new text values, from before and after the text change.

    When the editing is completed, the `OnEditorCompleted` method executes. This is achieved by unfocussing the [`Editor`](xref:Xamarin.Forms.Editor), or additionally by pressing the "Done" button on iOS. The `sender` argument is the `Editor` object responsible for firing the `TextChanged` event, and can be used to access the `Editor` object.

    > [!IMPORTANT]
    > Any text entered into an [`Editor`](xref:Xamarin.Forms.Editor) is stored in the [`Text`](xref:Xamarin.Forms.InputView.Text) property.

1. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS simulator or Android emulator:

    [![Screenshot of Editor containing text, on iOS and Android](../images/text-changes.png "Editor with text")](../images/text-changes-large.png#lightbox "Editor with text")

    Set breakpoints in the two event handlers, enter text into the [`Editor`](xref:Xamarin.Forms.Editor), and observe the [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) event firing. Unfocus the `Editor` to observe the [`Completed`](xref:Xamarin.Forms.Entry.Completed) event firing.

    For more information about [`Editor`](xref:Xamarin.Forms.Editor) events, see [Events and interactivity](~/xamarin-forms/user-interface/text/editor.md#events-and-interactivity) in the [Xamarin.Forms Editor](~/xamarin-forms/user-interface/text/editor.md) guide.
