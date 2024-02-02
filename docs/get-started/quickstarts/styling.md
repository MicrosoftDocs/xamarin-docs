---
title: "Style a cross-platform Xamarin.Forms application"
description: "This article explains how to style a cross-platform Xamarin.Forms Shell application with XAML styles, and use XAML Hot Reload to see these changes."
zone_pivot_groups: platform
ms.topic: quickstart
ms.service: xamarin
ms.assetid: 1C6ED8AF-41B4-4D6C-9BD8-C72D3F05E541
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/26/2021
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Style a cross-platform Xamarin.Forms application

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)

In this quickstart, you will learn how to:

- Style a Xamarin.Forms Shell application using XAML styles.
- Use XAML Hot Reload to see UI changes without rebuilding your application.

The quickstart walks through how to style a cross-platform Xamarin.Forms application with XAML styles. In addition, the quickstart uses XAML Hot Reload to update the UI of your running application, without having to rebuild the application. For more information about XAML Hot Reload, see [XAML Hot Reload for Xamarin.Forms](~/xamarin-forms/xaml/hot-reload.md).

The final application is shown below:

[![Notes Page](styling-images/screenshots1-sml.png)](styling-images/screenshots1.png#lightbox)
[![Note Entry Page](styling-images/screenshots2-sml.png)](styling-images/screenshots2.png#lightbox)
[![About Page](styling-images/screenshots3-sml.png)](styling-images/screenshots3.png#lightbox)

### Prerequisites

You should successfully complete the [previous quickstart](database.md) before attempting this quickstart. Alternatively, download the [previous quickstart sample](/samples/xamarin/xamarin-forms-samples/getstarted-notes-database/) and use it as the starting point for this quickstart.

::: zone pivot="windows"

## Update the app with Visual Studio

1. Launch Visual Studio and open the Notes solution.

2. Build and run the project on your chosen platform. For more information, see [Building the quickstart](app.md#building-the-quickstart).

    Leave the application running and return to Visual Studio.

3. In **Solution Explorer**, in the **Notes** project, open **App.xaml**. Then replace the existing code with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">

        <!-- Resources used by multiple pages in the application -->         
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppPrimaryColor">#1976D2</Color>
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="PrimaryColor">Black</Color>
            <Color x:Key="SecondaryColor">White</Color>
            <Color x:Key="TertiaryColor">Silver</Color>

            <!-- Implicit styles -->
            <Style TargetType="ContentPage"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button">
                <Setter Property="FontSize"
                        Value="Medium" />
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="TextColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="CornerRadius"
                        Value="5" />
            </Style>

        </Application.Resources>
    </Application>
    ```

    This code defines a [`Thickness`](xref:Xamarin.Forms.Thickness) value, a series of [`Color`](xref:Xamarin.Forms.Color) values, and implicit styles for the [`ContentPage`](xref:Xamarin.Forms.ContentPage) and [`Button`](xref:Xamarin.Forms.Button) types. Note that these styles, which are in the application-level [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), can be consumed throughout the application. For more information about XAML styling, see [Styling](deepdive.md#styling) in the [Xamarin.Forms Quickstart Deep Dive](deepdive.md).

    After making the changes to **App.xaml**, XAML Hot Reload will update the UI of the running app, with no need to rebuild the application. Specifically, the background color each page will change. By default Hot Reload applies changes immediately after stopping typing. However, there's a [preference setting](~/xamarin-forms/xaml/hot-reload.md) that can be changed, if you prefer, to wait until file save to apply changes.

4. In **Solution Explorer**, in the **Notes** project, open **AppShell.xaml**. Then replace the existing code with the following code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <Shell xmlns="http://xamarin.com/schemas/2014/forms"
           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
           xmlns:views="clr-namespace:Notes.Views"
           x:Class="Notes.AppShell">

        <Shell.Resources>
            <!-- Style Shell elements -->
            <Style x:Key="BaseStyle"
                   TargetType="Element">
                <Setter Property="Shell.BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="Shell.ForegroundColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TitleColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TabBarUnselectedColor"
                        Value="#95FFFFFF"/>
            </Style>
            <Style TargetType="TabBar"
                   BasedOn="{StaticResource BaseStyle}" />
        </Shell.Resources>

        <!-- Display a bottom tab bar containing two tabs -->   
        <TabBar>
            <ShellContent Title="Notes"
                          Icon="icon_feed.png"
                          ContentTemplate="{DataTemplate views:NotesPage}" />
            <ShellContent Title="About"
                          Icon="icon_about.png"
                          ContentTemplate="{DataTemplate views:AboutPage}" />
        </TabBar>
    </Shell>
    ```

    This code adds two styles to the `Shell` resource dictionary, which define a series of [`Color`](xref:Xamarin.Forms.Color) values used by the application.

    After making the **AppShell.xaml** changes, XAML Hot Reload will update the UI of the running app, without rebuilding the application. Specifically, the background color of the Shell chrome will change.

5. In **Solution Explorer**, in the **Notes** project, open **NotesPage.xaml** in the **Views** folder. Then replace the existing code with the following code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">

        <ContentPage.Resources>
            <!-- Define a visual state for the Selected state of the CollectionView -->
            <Style TargetType="StackLayout">
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal" />
                            <VisualState x:Name="Selected">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor"
                                            Value="{StaticResource AppPrimaryColor}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>
        </ContentPage.Resources>

        <!-- Add an item to the toolbar -->
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="Add"
                         Clicked="OnAddClicked" />
        </ContentPage.ToolbarItems>

        <!-- Display notes in a list -->
        <CollectionView x:Name="collectionView"
                        Margin="{StaticResource PageMargin}"
                        SelectionMode="Single"
                        SelectionChanged="OnSelectionChanged">
            <CollectionView.ItemsLayout>
                <LinearItemsLayout Orientation="Vertical"
                                   ItemSpacing="10" />
            </CollectionView.ItemsLayout>
            <!-- Define the appearance of each item in the list -->
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <StackLayout>
                        <Label Text="{Binding Text}"
                               FontSize="Medium" />
                        <Label Text="{Binding Date}"
                               TextColor="{StaticResource TertiaryColor}"
                               FontSize="Small" />
                    </StackLayout>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </ContentPage>
    ```

    This code adds an implicit style for the [`StackLayout`](xref:Xamarin.Forms.StackLayout) that defines the appearance of each selected item in the [`CollectionView`](xref:Xamarin.Forms.CollectionView), to the page-level [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), and sets the `CollectionView.Margin`  and `Label.TextColor` property to values defined in the application-level `ResourceDictionary`. Note that the `StackLayout` implicit style was added to the page-level `ResourceDictionary`, because it is only consumed by the `NotesPage`.

    After making the **NotesPage.xaml** changes, XAML Hot Reload will update the UI of the running app, without rebuilding the application. Specifically, the color of selected items in the [`CollectionView`](xref:Xamarin.Forms.CollectionView) will change.

6. In **Solution Explorer**, in the **Notes** project, open **NoteEntryPage.xaml** in the **Views** folder. Then replace the existing code with the following code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>         
        </ContentPage.Resources>

        <!-- Layout children vertically -->
        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
                    HeightRequest="100" />
            <Grid ColumnDefinitions="*,*">
                <!-- Layout children in two columns -->
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    This code adds an implicit style for the [`Editor`](xref:Xamarin.Forms.Editor) to the page-level [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), and sets the `StackLayout.Margin` property to a value defined in the application-level `ResourceDictionary`. Note that the `Editor` implicit styles was added to the page-level `ResourceDictionary` because it's only consumed by the `NoteEntryPage`.

7. In the running application, navigate to the `NoteEntryPage`.

    XAML Hot Reload updated the UI of the application, without rebuilding it. Specifically, the background color of the [`Editor`](xref:Xamarin.Forms.Editor) changed in the running application, as well as the appearance of the [`Button`](xref:Xamarin.Forms.Button) objects.

8. In **Solution Explorer**, in the **Notes** project, open **AboutPage.xaml** in the **Views** folder. Then replace the existing code with the following code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.AboutPage"
                 Title="About">
        <!-- Layout children in two rows -->
        <Grid RowDefinitions="Auto,*">
            <Image Source="xamarin_logo.png"
                   BackgroundColor="{StaticResource AppPrimaryColor}"
                   Opacity="0.85"
                   VerticalOptions="Center"
                   HeightRequest="64" />
            <!-- Layout children vertically -->       
            <StackLayout Grid.Row="1"
                         Margin="{StaticResource PageMargin}"
                         Spacing="20">
                <Label FontSize="22">
                    <Label.FormattedText>
                        <FormattedString>
                            <FormattedString.Spans>
                                <Span Text="Notes"
                                      FontAttributes="Bold"
                                      FontSize="22" />
                                <Span Text=" v1.0" />
                            </FormattedString.Spans>
                        </FormattedString>
                    </Label.FormattedText>
                </Label>
                <Label Text="This app is written in XAML and C# with the Xamarin Platform." />
                <Button Text="Learn more"
                        Clicked="OnButtonClicked" />
            </StackLayout>
        </Grid>
    </ContentPage>
    ```

    This code sets the `Image.BackgroundColor` and `StackLayout.Margin` properties to values defined in the application-level [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary).

9. In the running application, navigate to the `AboutPage`.

    XAML Hot Reload updated the UI of the application, without rebuilding it. Specifically, the background color of the [`Image`](xref:Xamarin.Forms.Editor) changed in the running application.

::: zone-end
::: zone pivot="macos"

## Update the app with Visual Studio for Mac

1. Launch Visual Studio for Mac and open the Notes project.

2. Build and run the project on your chosen platform. For more information, see [Building the quickstart](app.md#building-the-quickstart).

    Leave the application running and return to Visual Studio for Mac.

3. In the **Solution Pad**, in the **Notes** project, open **App.xaml**. Then replace the existing code with the following code:

    ```xaml
    <?xml version="1.0" encoding="utf-8" ?>
    <Application xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.App">

        <!-- Resources used by multiple pages in the application -->                 
        <Application.Resources>

            <Thickness x:Key="PageMargin">20</Thickness>

            <!-- Colors -->
            <Color x:Key="AppPrimaryColor">#1976D2</Color>
            <Color x:Key="AppBackgroundColor">AliceBlue</Color>
            <Color x:Key="PrimaryColor">Black</Color>
            <Color x:Key="SecondaryColor">White</Color>
            <Color x:Key="TertiaryColor">Silver</Color>

            <!-- Implicit styles -->
            <Style TargetType="ContentPage"
                   ApplyToDerivedTypes="True">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>

            <Style TargetType="Button">
                <Setter Property="FontSize"
                        Value="Medium" />
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="TextColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="CornerRadius"
                        Value="5" />
            </Style>  
        </Application.Resources>
    </Application>
    ```

    This code defines a [`Thickness`](xref:Xamarin.Forms.Thickness) value, a series of [`Color`](xref:Xamarin.Forms.Color) values, and implicit styles for the [`ContentPage`](xref:Xamarin.Forms.ContentPage) and [`Button`](xref:Xamarin.Forms.Button) types. Note that these styles, which are in the application-level [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), can be consumed throughout the application. For more information about XAML styling, see [Styling](deepdive.md#styling) in the [Xamarin.Forms Quickstart Deep Dive](deepdive.md).

    After making the changes to **App.xaml**, XAML Hot Reload will update the UI of the running app, with no need to rebuild the application. Specifically, the background color each page will change. By default Hot Reload applies changes immediately after stopping typing. However, there's a [preference setting](~/xamarin-forms/xaml/hot-reload.md) that can be changed, if you prefer, to wait until file save to apply changes. 

4. In the **Solution Pad**, in the **Notes** project, open **AppShell.xaml**. Then replace the existing code with the following code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <Shell xmlns="http://xamarin.com/schemas/2014/forms"
           xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
           xmlns:views="clr-namespace:Notes.Views"
           x:Class="Notes.AppShell">

        <Shell.Resources>
            <!-- Style Shell elements -->
            <Style x:Key="BaseStyle"
                   TargetType="Element">
                <Setter Property="Shell.BackgroundColor"
                        Value="{StaticResource AppPrimaryColor}" />
                <Setter Property="Shell.ForegroundColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TitleColor"
                        Value="{StaticResource SecondaryColor}" />
                <Setter Property="Shell.TabBarUnselectedColor"
                        Value="#95FFFFFF"/>
            </Style>
            <Style TargetType="TabBar"
                   BasedOn="{StaticResource BaseStyle}" />
        </Shell.Resources>

        <!-- Display a bottom tab bar containing two tabs -->
        <TabBar>
            <ShellContent Title="Notes"
                          Icon="icon_feed.png"
                          ContentTemplate="{DataTemplate views:NotesPage}" />
            <ShellContent Title="About"
                          Icon="icon_about.png"
                          ContentTemplate="{DataTemplate views:AboutPage}" />
        </TabBar>
    </Shell>
    ```

    This code adds two styles to the `Shell` resource dictionary, which define a series of [`Color`](xref:Xamarin.Forms.Color) values used by the application.

    After making the **AppShell.xaml** changes, XAML Hot Reload will update the UI of the running app, without rebuilding the application. Specifically, the background color of the Shell chrome will change.

5. In the **Solution Pad**, in the **Notes** project, open **NotesPage.xaml** in the **Views** folder. Then replace the existing code with the following code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NotesPage"
                 Title="Notes">

        <ContentPage.Resources>
            <!-- Define a visual state for the Selected state of the CollectionView -->
            <Style TargetType="StackLayout">
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal" />
                            <VisualState x:Name="Selected">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor"
                                            Value="{StaticResource AppPrimaryColor}" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>
        </ContentPage.Resources>

        <!-- Add an item to the toolbar -->
        <ContentPage.ToolbarItems>
            <ToolbarItem Text="Add"
                         Clicked="OnAddClicked" />
        </ContentPage.ToolbarItems>

        <!-- Display notes in a list -->
        <CollectionView x:Name="collectionView"
                        Margin="{StaticResource PageMargin}"
                        SelectionMode="Single"
                        SelectionChanged="OnSelectionChanged">
            <CollectionView.ItemsLayout>
                <LinearItemsLayout Orientation="Vertical"
                                   ItemSpacing="10" />
            </CollectionView.ItemsLayout>
            <!-- Define the appearance of each item in the list -->
            <CollectionView.ItemTemplate>
                <DataTemplate>
                    <StackLayout>
                        <Label Text="{Binding Text}"
                               FontSize="Medium" />
                        <Label Text="{Binding Date}"
                               TextColor="{StaticResource TertiaryColor}"
                               FontSize="Small" />
                    </StackLayout>
                </DataTemplate>
            </CollectionView.ItemTemplate>
        </CollectionView>
    </ContentPage>
    ```

    This code adds an implicit style for the [`StackLayout`](xref:Xamarin.Forms.StackLayout) that defines the appearance of each selected item in the [`CollectionView`](xref:Xamarin.Forms.CollectionView), to the page-level [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), and sets the `CollectionView.Margin`  and `Label.TextColor` property to values defined in the application-level `ResourceDictionary`. Note that the `StackLayout` implicit style was added to the page-level `ResourceDictionary`, because it is only consumed by the `NotesPage`.

    After making the **NotesPage.xaml** changes, XAML Hot Reload will update the UI of the running app, without rebuilding the application. Specifically, the color of selected items in the [`CollectionView`](xref:Xamarin.Forms.CollectionView) will change.

6. In the **Solution Pad**, in the **Notes** project, open **NoteEntryPage.xaml** in the **Views** folder. Then replace the existing code with the following code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.NoteEntryPage"
                 Title="Note Entry">
        <ContentPage.Resources>
            <!-- Implicit styles -->
            <Style TargetType="{x:Type Editor}">
                <Setter Property="BackgroundColor"
                        Value="{StaticResource AppBackgroundColor}" />
            </Style>       
        </ContentPage.Resources>

        <!-- Layout children vertically -->
        <StackLayout Margin="{StaticResource PageMargin}">
            <Editor Placeholder="Enter your note"
                    Text="{Binding Text}"
                    HeightRequest="100" />
            <!-- Layout children in two columns -->
            <Grid ColumnDefinitions="*,*">
                <Button Text="Save"
                        Clicked="OnSaveButtonClicked" />
                <Button Grid.Column="1"
                        Text="Delete"
                        Clicked="OnDeleteButtonClicked"/>
            </Grid>
        </StackLayout>
    </ContentPage>
    ```

    This code adds implicit styles for the [`Editor`](xref:Xamarin.Forms.Editor) to the page-level [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), and sets the `StackLayout.Margin` property to a value defined in the application-level `ResourceDictionary`. Note that the `Editor` implicit style was added to the page-level `ResourceDictionary` because it's only consumed by the `NoteEntryPage`.

7. In the running application, navigate to the `NoteEntryPage`.

    XAML Hot Reload updated the UI of the application, without rebuilding it. Specifically, the background color of the [`Editor`](xref:Xamarin.Forms.Editor) changed in the running application, as well as the appearance of the [`Button`](xref:Xamarin.Forms.Button) objects.

8. In the **Solution Pad**, in the **Notes** project, open **AboutPage.xaml** in the **Views** folder. Then replace the existing code with the following code:

    ```xaml
    <?xml version="1.0" encoding="UTF-8"?>
    <ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
                 xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                 x:Class="Notes.Views.AboutPage"
                 Title="About">
        <!-- Layout children in two rows -->
        <Grid RowDefinitions="Auto,*">
            <Image Source="xamarin_logo.png"
                   BackgroundColor="{StaticResource AppPrimaryColor}"
                   Opacity="0.85"
                   VerticalOptions="Center"
                   HeightRequest="64" />
            <!-- Layout children vertically -->
            <StackLayout Grid.Row="1"
                         Margin="{StaticResource PageMargin}"
                         Spacing="20">
                <Label FontSize="22">
                    <Label.FormattedText>
                        <FormattedString>
                            <FormattedString.Spans>
                                <Span Text="Notes"
                                      FontAttributes="Bold"
                                      FontSize="22" />
                                <Span Text=" v1.0" />
                            </FormattedString.Spans>
                        </FormattedString>
                    </Label.FormattedText>
                </Label>
                <Label Text="This app is written in XAML and C# with the Xamarin Platform." />
                <Button Text="Learn more"
                        Clicked="OnButtonClicked" />
            </StackLayout>
        </Grid>
    </ContentPage>
    ```

    This code sets the `Image.BackgroundColor` and `StackLayout.Margin` properties to values defined in the application-level [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary).

9. In the running application, navigate to the `AboutPage`.

    XAML Hot Reload updated the UI of the application, without rebuilding it. Specifically, the background color of the [`Image`](xref:Xamarin.Forms.Editor) changed in the running application.

::: zone-end

## Next steps

In this quickstart, you learned how to:

- Style a Xamarin.Forms Shell application using XAML styles.
- Use XAML Hot Reload to see UI changes without rebuilding your application.

To learn more about the fundamentals of application development using Xamarin.Forms Shell, continue to the quickstart deep dive.

> [!div class="nextstepaction"]
> [Next](deepdive.md)

## Related links

- [Notes (sample)](/samples/xamarin/xamarin-forms-samples/getstarted-notes-styled/)
- [XAML Hot Reload for Xamarin.Forms](~/xamarin-forms/xaml/hot-reload.md)
- [Xamarin.Forms Quickstart Deep Dive](deepdive.md)
