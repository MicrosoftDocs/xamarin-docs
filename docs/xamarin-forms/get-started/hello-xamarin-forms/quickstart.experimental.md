---
title: "Xamarin.Forms Quickstart"
description: "This article explains how to create a simple cross-platform Notes application, using Xamarin.Forms, that allows you to enter a note and persist it to device storage."
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E8CF05B1-54B9-428B-8518-D068837BD61E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/21/2018
experimental: false
experiment_id: 06675725-079f-48
---

# Xamarin.Forms Quickstart

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)

This quickstart walks through how to create a simple cross-platform Notes application, using Xamarin.Forms, that allows you to enter a note and persist it to device storage. The final application is shown below:

[![](quickstart-experimental-images/screenshots-sml.png "Notes Application")](quickstart-experimental-images/screenshots.png#lightbox "Notes Application")

::: zone pivot="windows"

## Get started with Visual Studio

1. In the **Start** screen, launch Visual Studio. This opens the start page:

    ![](quickstart-experimental-images/vs/start-page.png "Visual Studio")

2. In Visual Studio, click **Create new project...** to create a new project:

    ![](quickstart-experimental-images/vs/new-solution.png "New Project")

3. In the **New Project** dialog, click **Cross-Platform**, select the **Mobile App (Xamarin.Forms)** template, set the Name to **Notes**, choose a suitable location for the project and click the **OK** button:

    ![](quickstart-experimental-images/vs/new-project.png "Cross-Platform Project Templates")

    > [!IMPORTANT]
    > The C# and XAML snippets in this quickstart requires that the solution is named **Notes**. Using a different name will result in build errors when you copy code from this quickstart into the solution.

4. In the **New Cross Platform App** dialog, click **Blank App**, select **.NET Standard** as the Code Sharing Strategy, and click the **OK** button:

    ![](quickstart-experimental-images/vs/new-app.png "New Cross-Platform App")

5. In **Solution Explorer**, in the **Notes** project, double-click **MainPage.xaml** to open it:

    ![](quickstart-experimental-images/vs/open-mainpage-xaml.png "Open MainPage.xaml")

6. In **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="_editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of a [`Label`](xref:Xamarin.Forms.Label) to display text, a [`Editor`](xref:Xamarin.Forms.Editor) for text input, and two [`Button`](xref:Xamarin.Forms.Button) instances that direct the application to save or delete a file. The two `Button` instances are horizontally laid out in a [`Grid`](xref:Xamarin.Forms.Grid), with the `Label`, `Editor`, and `Grid` being vertically laid out in a [`StackLayout`](xref:Xamarin.Forms.StackLayout).

    Save the changes to **MainPage.xaml** by pressing **CTRL+S**, and close the file.

7. In **Solution Explorer**, in the **Notes** project, expand **MainPage.xaml** and double-click **MainPage.xaml.cs** to open it:

    ![](quickstart-experimental-images/vs/open-mainpage-codebehind.png "Open MainPage.xaml.cs")

8. In **MainPage.xaml.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    _editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, _editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                _editor.Text = string.Empty;
            }
        }
    }
    ```

    This code defines a `_fileName` field, which references a file named `notes.txt` that will store note data in the local application data folder for the application. When the page constructor is executed the file is read, if it exists, and displayed in the [`Editor`](xref:Xamarin.Forms.Editor). When the **Save** [`Button`](xref:Xamarin.Forms.Button) is pressed the `OnSaveButtonClicked` event handler is executed, which saves the content of the `Editor` to the file. When the **Delete** `Button` is pressed the `OnDeleteButtonClicked` event handler is executed, which deletes the file, provided that it exists, and removes any text from the `Editor`.

    Save the changes to **MainPage.xaml.cs** by pressing **CTRL+S**, and close the file.

### Building the quickstart

1. In Visual Studio, select the **Build > Build Solution** menu item (or press F6). The solution will build and a success message will appear in the Visual Studio status bar:

      ![](quickstart-experimental-images/vs/build-succeeded.png "Build Succeeded")

    If there are errors, repeat the previous steps and correct any mistakes until the solution builds successfully.

2. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application in your chosen Android emulator:

    ![](quickstart-experimental-images/vs/android-start.png "Visual Studio Android Toolbar")

    ![](quickstart-experimental-images/vs/notes-android.png "Notes in the Android emulator")

    Enter a note and press the **Save** button.

    > [!NOTE]
    > The following steps should only be carried out if you have a [paired Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) that meets the system requirements for Xamarin.Forms development.

3. In the Visual Studio toolbar, right-click on the **Notes.iOS** project, and select **Set as StartUp Project**.

      ![](quickstart-experimental-images/vs/set-as-startup-project-ios.png "Set iOS as Startup Project")

4. In the Visual Studio toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application in your chosen [iOS remote simulator](~/tools/ios-simulator/index.md):

    ![](quickstart-experimental-images/vs/ios-start.png "Visual Studio iOS Toolbar")

    ![](quickstart-experimental-images/vs/notes-ios.png "Notes in the iOS Simulator")

    Enter a note and press the **Save** button.

::: zone-end
::: zone pivot="macos"

## Get started with Visual Studio for Mac

1. Launch Visual Studio for Mac, and on the start page click **New Project...** to create a new project:

    ![](quickstart-experimental-images/vsmac/new-project.png "New Solution")

2. In the **Choose a template for your new project** dialog, click **Multiplatform > App**, select the **Blank Forms App** template, and click the **Next** button:

    ![](quickstart-experimental-images/vsmac/choose-template.png "Choose a Template")

3. In the **Configure your Blank Forms app** dialog, name the new app **Notes**, ensure that the **Use .NET Standard** radio button is selected, and click the **Next** button:    

    ![](quickstart-experimental-images/vsmac/configure-app.png "Configure the Forms Application")

4. In the **Configure your new Blank Forms app** dialog, leave the Solution and Project names set to **Notes**, choose a suitable location for the project, and click the **Create** button to create the project:

    ![](quickstart-experimental-images/vsmac/configure-project.png "Configure the Forms Project")

    > [!IMPORTANT]
    > The C# and XAML snippets in this quickstart requires that the solution and project are both named **Notes**. Using a different name will result in build errors when you copy code from this quickstart into the project.

5. In the **Solution Pad**, in the **Notes** project, double-click **MainPage.xaml** to open it:

    ![](quickstart-experimental-images/vsmac/mainpage-xaml.png "MainPage.xaml")

6. In **MainPage.xaml**, remove all of the template code and replace it with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.MainPage">
        <StackLayout Margin="10,35,10,10">
            <Label Text="Notes"
                   HorizontalOptions="Center"
                   FontAttributes="Bold" />
            <Editor x:Name="_editor"
                    Placeholder="Enter your note"
                    HeightRequest="100" />
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    This code declaratively defines the user interface for the page, which consists of a [`Label`](xref:Xamarin.Forms.Label) to display text, a [`Editor`](xref:Xamarin.Forms.Editor) for text input, and two [`Button`](xref:Xamarin.Forms.Button) instances that direct the application to save or delete a file. The two `Button` instances are horizontally laid out in a [`Grid`](xref:Xamarin.Forms.Grid), with the `Label`, `Editor`, and `Grid` being vertically laid out in a [`StackLayout`](xref:Xamarin.Forms.StackLayout).

    Save the changes to **MainPage.xaml** by choosing **File > Save** (or by pressing **&#8984; + S**), and close the file.

7. In the **Solution Pad**, in the **Notes** project, expand **MainPage.xaml** and double-click **MainPage.xaml.cs** to open it:

    ![](quickstart-experimental-images/vsmac/mainpage-xaml-cs.png "MainPage.xaml.cs")

8. In **MainPage.xaml.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace Notes
    {
        public partial class MainPage : ContentPage
        {
            string _fileName = Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "notes.txt");

            public MainPage()
            {
                InitializeComponent();

                if (File.Exists(_fileName))
                {
                    _editor.Text = File.ReadAllText(_fileName);
                }
            }

            void OnSaveButtonClicked(object sender, EventArgs e)
            {
                File.WriteAllText(_fileName, _editor.Text);
            }

            void OnDeleteButtonClicked(object sender, EventArgs e)
            {
                if (File.Exists(_fileName))
                {
                    File.Delete(_fileName);
                }
                _editor.Text = string.Empty;
            }
        }
    }
    ```

    This code defines a `_fileName` field, which references a file named `notes.txt` that will store note data in the local application data folder for the application. When the page constructor is executed the file is read, if it exists, and displayed in the [`Editor`](xref:Xamarin.Forms.Editor). When the **Save** [`Button`](xref:Xamarin.Forms.Button) is pressed the `OnSaveButtonClicked` event handler is executed, which saves the content of the `Editor` to the file. When the **Delete** `Button` is pressed the `OnDeleteButtonClicked` event handler is executed, which deletes the file, provided that it exists, and removes any text from the `Editor`.

    Save the changes to **MainPage.xaml.cs** by choosing **File > Save** (or by pressing **&#8984; + S**), and close the file.

### Building the quickstart

1. In Visual Studio for Mac, select the **Build > Build All** menu item (or press **&#8984; + B**). The projects will build and a success message will appear in the Visual Studio for Mac toolbar.

      ![](quickstart-experimental-images/vsmac/build-successful.png "Build Successful")

    If there are errors, repeat the previous steps and correct any mistakes until the projects build successfully.

2. In the **Solution Pad**, select the **Notes.iOS** project, right-click and select **Set As Startup Project**:

      ![](quickstart-experimental-images/vsmac/set-startup-project-ios.png "Set iOS as Startup Project")

3. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen iOS Simulator:

      ![](quickstart-experimental-images/vsmac/start.png "Visual Studio for Mac Toolbar")

      ![](quickstart-experimental-images/vsmac/notes-ios.png "Notes in the iOS Simulator")

    Enter a note and press the **Save** button.

4. In the **Solution Pad**, select the **Notes.Droid** project, right-click and select **Set As Startup Project**:

      ![](quickstart-experimental-images/vsmac/set-startup-project-android.png "Set Android as Startup Project")

5. In the Visual Studio for Mac toolbar, press the **Start** button (the triangular button that resembles a Play button) to launch the application inside your chosen Android emulator:

      ![](quickstart-experimental-images/vsmac/notes-android.png "Notes in the Android Emulator")

    Enter a note and press the **Save** button.

::: zone-end

## Related Links

- [Notes (sample)](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/Notes/SinglePage/)
