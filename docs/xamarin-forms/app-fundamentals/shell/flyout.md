---
title: "Xamarin.Forms Shell Flyout"
description: "The flyout is the root menu for a Shell application, and is accessible through an icon or by swiping from the side of the screen. The flyout consists of an optional header, flyout items, and optional menu items."
ms.prod: xamarin
ms.assetid: FEDE51EB-577E-4B3E-9890-B7C1A5E52516
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/05/2019
---

# Xamarin.Forms Shell Flyout

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

The flyout is the root menu for a Shell application, and is accessible through an icon or by swiping from the side of the screen. The flyout consists of an optional header, flyout items, and optional menu items:

![Screenshot of a Shell annotated flyout](flyout-images/flyout-annotated.png "Annotated flyout")

If required, the background color of the flyout can be set to a [`Color`](xref:Xamarin.Forms.Color) through the `Shell.FlyoutBackgroundColor` bindable property. This property can also be set from a Cascading Style Sheet (CSS). For more information, see [Xamarin.Forms Shell specific properties](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## Flyout icon

By default, Shell applications have a hamburger icon which, when pressed, opens the flyout. This icon can be changed by setting the `Shell.FlyoutIcon` bindable property, of type [`ImageSource`](xref:Xamarin.Forms.ImageSource), to an appropriate icon:

```xaml
<Shell ...
       FlyoutIcon="flyouticon.png">
    ...       
</Shell>
```

## Flyout behavior

The flyout can be accessed through the hamburger icon or by swiping from the side of the screen. However, this behavior can be changed by setting the `Shell.FlyoutBehavior` attached property to one of the `FlyoutBehavior` enumeration members:

- `Disabled` – indicates that the flyout can't be opened by the user.
- `Flyout` – indicates that the flyout can be opened and closed by the user. This is the default value for the `FlyoutBehavior` property.
- `Locked` – indicates that the flyout can't be closed by the user, and that it doesn't overlap content.

The following example shows how to disable the flyout:

```xaml
<Shell ...
       FlyoutBehavior="Disabled">
    ...
</Shell>
```

> [!NOTE]
> The `FlyoutBehavior` attached property can be set on `Shell`, `FlyoutItem`, `ShellContent`, and page objects, to override the default flyout behavior.

In addition, the flyout can be programmatically opened and closed by setting the `Shell.FlyoutIsPresented` bindable property to a `boolean` value that indicates whether the flyout is currently visible:

```csharp
Shell.Current.FlyoutIsPresented = false;
```

## Flyout header

The flyout header is the content that optionally appears at the top of the flyout, with its appearance being defined by an `object` that can be set through the `Shell.FlyoutHeader` property value:

```xaml
<Shell.FlyoutHeader>
    <controls:FlyoutHeader />
</Shell.FlyoutHeader>
```

The `FlyoutHeader` type is shown in the following example:

```xaml
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Xaminals.Controls.FlyoutHeader"
             HeightRequest="200">
    <Grid BackgroundColor="Black">
        <Image Aspect="AspectFill"
               Source="xamarinstore.jpg"
               Opacity="0.6" />
        <Label Text="Animals"
               TextColor="White"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentView>
```

This results in the following flyout header:

![Screenshot of the flyout header](flyout-images/flyout-header.png "Flyout header")

Alternatively, the flyout header appearance can be defined by setting the `Shell.FlyoutHeaderTemplate` property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell.FlyoutHeaderTemplate>
    <DataTemplate>
        <Grid BackgroundColor="Black"
              HeightRequest="200">
            <Image Aspect="AspectFill"
                   Source="xamarinstore.jpg"
                   Opacity="0.6" />
            <Label Text="Animals"
                   TextColor="White"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center" />
        </Grid>            
    </DataTemplate>
</Shell.FlyoutHeaderTemplate>
```

By default, the flyout header will be fixed in the flyout while the content below will scroll if there are enough items. However, this behavior can be changed by setting the `Shell.FlyoutHeaderBehavior` bindable property to one of the `FlyoutHeaderBehavior` enumeration members:

- `Default` – indicates that the default behavior for the platform will be used. This is the default value of the `FlyoutHeaderBehavior` property.
- `Fixed` – indicates that the flyout header remains visible and unchanged at all times.
- `Scroll` – indicates that the flyout header scrolls out of view as the user scrolls the items.
- `CollapseOnScroll` – indicates that the flyout header collapses to a title only, as the user scrolls the items.

The following example shows how to collapse the flyout header as the user scrolls:

```xaml
<Shell ...
       FlyoutHeaderBehavior="CollapseOnScroll">
    ...
</Shell>
```

## Flyout background image

The flyout can have an optional background image, which appears beneath the flyout header and behind any flyout items and menu items. The background image can be specified by setting the `FlyoutBackgroundImage` bindable property, of type [`ImageSource`](xref:Xamarin.Forms.ImageSource), to a file, embedded resource, URI, or stream.

The aspect ratio of the background image can be configured by setting the `FlyoutBackgroundImageAspect` bindable property, of type [`Aspect`](xref:Xamarin.Forms.Aspect), to one of the `Aspect` enumeration members:

- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) - clips the image so that it fills the display area while preserving the aspect ratio.
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) - letterboxes the image, if required, so that the image fits into the display area, with blank space added to the top/bottom or sides depending on whether the image is wide or tall.
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) - stretches the image to completely and exactly fill the display area. This may result in image distortion.

By default, the `FlyoutBackgroundImageAspect` property will be set to `AspectFit`.

The following example shows setting these properties:

```xaml
<Shell ...
       FlyoutBackgroundImage="photo.jpg"
       FlyoutBackgroundImageAspect="AspectFill">
    ...
</Shell>
```

This results in a background image appearing in the flyout:

![Screenshot of a flyout background image](flyout-images/flyout-backgroundimage.png "Flyout background image")

## Flyout items

When the navigation pattern for an application includes a flyout, the subclassed `Shell` object must contain one or more `FlyoutItem` objects, with each `FlyoutItem` object representing an item on the flyout. Each `FlyoutItem` object should be a child of the `Shell` object.

The following example creates a flyout containing a flyout header and two flyout items:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <FlyoutItem Title="Cats"
                Icon="cat.png">
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
    <FlyoutItem Title="Dogs"
                Icon="dog.png">
        <Tab>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

In this example, each [`ContentPage`](xref:Xamarin.Forms.ContentPage) can only be accessed through flyout items:

[![Screenshot of a Shell two page app with flyout items, on iOS and Android](flyout-images/two-page-app-flyout.png "Shell two page app with flyout items")](flyout-images/two-page-app-flyout-large.png#lightbox "Shell two page app with flyout items")

> [!NOTE]
> When a flyout header isn't present, flyout items appear at the top of the flyout. Otherwise, they appear below the flyout header.

Shell has implicit conversion operators that enable the Shell visual hierarchy to be simplified, without introducing additional views into the visual tree. This is possible because a subclassed `Shell` object can only ever contain `FlyoutItem` objects or a `TabBar` object, which can only ever contain `Tab` objects, which can only ever contain `ShellContent` objects. These implicit conversion operators can be used to remove the `FlyoutItem`, `Tab`, and `ShellContent` objects from the previous example:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <views:CatsPage IconImageSource="cat.png" />
    <views:DogsPage IconImageSource="dog.png" />
</Shell>
```

This implicit conversion automatically wraps each [`ContentPage`](xref:Xamarin.Forms.ContentPage) object in `ShellContent` objects, which are wrapped in `Tab` objects, which are wrapped in `FlyoutItem` objects.

> [!IMPORTANT]
> In a Shell application, each [`ContentPage`](xref:Xamarin.Forms.ContentPage) that's a child of a `ShellContent` object is created during application startup. Adding additional `ShellContent` objects using this approach will result in additional pages being created during application startup, which can lead to a poor startup experience. However, Shell is also capable of creating pages on demand, in response to navigation. For more information, see [Efficient page loading](tabs.md#efficient-page-loading) in the [Xamarin.Forms Shell Tabs](tabs.md) guide.

### FlyoutItem class

The `FlyoutItem` class includes the following properties that control flyout item appearance and behavior:

- `FlyoutDisplayOptions`, of type `FlyoutDisplayOptions`, defines how the item and its children are displayed in the flyout. The default value is `AsSingleItem`.
- `CurrentItem`, of type `Tab`, the selected item.
- `Items`, of type `IList<Tab>`, defines all of the tabs within a `FlyoutItem`.
- `FlyoutIcon`, of type `ImageSource`, the icon to use for the item. If this property is unset, it will fallback to using the `Icon` property value.
- `Icon`, of type `ImageSource`, defines the icon to display in parts of the chrome that are not the flyout.
- `IsChecked`, of type `boolean`, defines if the item is currently highlighted in the flyout.
- `IsEnabled`, of type `boolean`, defines if the item is selectable in the chrome.
- `IsTabStop`, of type `bool`, indicates whether a `FlyoutItem` is included in tab navigation. Its default value is `true`, and when its value is `false` the `FlyoutItem` is ignored by the tab-navigation infrastructure, irrespective if a `TabIndex` is set.
- `TabIndex`, of type `int`, indicates the order in which `FlyoutItem` objects receive focus when the user navigates through items by pressing the Tab key. The default value of the property is 0.
- `Title`, of type `string`, the title to display in the UI.
- `Route`, of type `string`, the string used to address the item.

All of these properties, except the `Route` property, are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that the properties can be targets of data bindings.

> [!NOTE]
> All `FlyoutItem` objects in a subclassed Shell object are added to the `Shell.Items` collection, which defines the list of items that will be shown in the flyout.

In addition, the `FlyoutItem` class exposes the following overridable methods:

- `OnTabIndexPropertyChanged`, that's called whenever the `TabIndex` property changes.
- `OnTabStopPropertyChanged`, that's called whenever the `IsTabStop` property changes.
- `TabIndexDefaultValueCreator`, returns an `int`, and is called to set the default value of the `TabIndex` property.
- `TabStopDefaultValueCreator`, returns a `bool`, and is called to set the default value of the `TabStop` property.

## Flyout vertical scroll

By default, a flyout can be scrolled vertically when the flyout items don't fit in the flyout. This behavior can be changed by setting the `Shell.FlyoutVerticalScrollMode` bindable property to one of the `ScrollMode` enumeration members:

- `Disabled` – indicates that vertical scrolling will be disabled.
- `Enabled` – indicates that vertical scrolling will be enabled.
- `Auto` – indicates that vertical scrolling will be enabled if the flyout items don't fit in the flyout. This is the default value of the `Shell.FlyoutVerticalScrollMode` property.

The following example shows how to disable vertical scrolling:

```xaml
<Shell ...
       FlyoutVerticalScrollMode="Disabled"
    ...
</Shell>
```

## Flyout display options

The `FlyoutDisplayOptions` enumeration defines the following members:

- `AsSingleItem`, indicates that the item will be visible as a single item.
- `AsMultipleItems`, indicates that the item and its children will be visible in the flyout as a group of items.

By setting the `FlyoutItem.FlyoutDisplayOptions` property to `AsMultipleItems`, a flyout item for each `Tab` within a `FlyoutItem` will be created:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       FlyoutHeaderBehavior="CollapseOnScroll"
       x:Class="Xaminals.AppShell">

    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>

    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png"
                          ContentTemplate="{DataTemplate views:CatsPage}" />
            <ShellContent Title="Dogs"
                          Icon="dog.png"
                          ContentTemplate="{DataTemplate views:DogsPage}" />
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png"
                      ContentTemplate="{DataTemplate views:MonkeysPage}" />
        <ShellContent Title="Elephants"
                      Icon="elephant.png"
                      ContentTemplate="{DataTemplate views:ElephantsPage}" />  
        <ShellContent Title="Bears"
                      Icon="bear.png"
                      ContentTemplate="{DataTemplate views:BearsPage}" />
    </FlyoutItem>

    <ShellContent Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />    
</Shell>
```

In this example, flyout items are created for the `Tab` object that's a child of the `FlyoutItem` object, and the `ShellContent` objects that are children of the `FlyoutItem` object. This occurs because each `ShellContent` object that's a child of the `FlyoutItem` object is automatically wrapped in a `Tab` object. In addition, a flyout item is created for the final `ShellContent` object, which is wrapped in a `Tab` object, and then in a `FlyoutItem` object.

This results in the following flyout items:

[![Screenshot of flyout containing FlyoutItem objects, on iOS and Android](flyout-images/flyout-reduced.png "Shell flyout containing FlyoutItem objects")](flyout-images/flyout-reduced-large.png#lightbox "Shell flyout containing FlyoutItem objects")

## Define FlyoutItem appearance

The appearance of each `FlyoutItem` can be customized by setting the `Shell.ItemTemplate` attached property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    ...
    <Shell.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding FlyoutIcon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Title}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.ItemTemplate>
</Shell>
```

This example displays the title of each `FlyoutItem` object in italics:

[![Screenshot of templated FlyoutItem objects, on iOS and Android](flyout-images/flyoutitem-templated.png "Shell templated FlyoutItem objects")](flyout-images/flyoutitem-templated-large.png#lightbox "Shell templated FlyoutItem objects")


Because `Shell.ItemTemplate` is an attached property, different templates can be attached to specific `FlyoutItem` objects.

> [!NOTE]
> Shell provides the `Title` and `FlyoutIcon` properties to the [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) of the `ItemTemplate`.


### Default Template for FlyoutItems and MenuItems
Shell uses the following template internally for its default implementation. This is a great starting point if all you want to do is make small tweaks to the existing layouts. This also demonstrates the Visual State Manager features of the flyout items. This same template can also be used for MenuItems

```xaml
<DataTemplate x:Key="FlyoutTemplates">
    <Grid HeightRequest="{x:OnPlatform Android=50}">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroupList>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal">
                    </VisualState>
                    <VisualState x:Name="Selected">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="#F2F2F2" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateGroupList>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="{x:OnPlatform Android=54, iOS=50}"></ColumnDefinition>
            <ColumnDefinition Width="*"></ColumnDefinition>
        </Grid.ColumnDefinitions>
        <Image Source="{Binding FlyoutIcon}"
            VerticalOptions="Center"
            HorizontalOptions="Center"
            HeightRequest="{x:OnPlatform Android=24, iOS=22}"
            WidthRequest="{x:OnPlatform Android=24, iOS=22}">
        </Image>
        <Label VerticalOptions="Center"
                Text="{Binding Title}"
                FontSize="{x:OnPlatform Android=14, iOS=Small}"
                FontAttributes="Bold" Grid.Column="1">
            <Label.TextColor>
                <OnPlatform x:TypeArguments="Color">
                    <OnPlatform.Platforms>
                        <On Platform="Android" Value="#D2000000" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.TextColor>
            <Label.Margin>
                <OnPlatform x:TypeArguments="Thickness">
                    <OnPlatform.Platforms>
                        <On Platform="Android" Value="20, 0, 0, 0" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.Margin>
            <Label.FontFamily>
                <OnPlatform x:TypeArguments="x:String">
                    <OnPlatform.Platforms>
                        <On Platform="Android" Value="sans-serif-medium" />
                    </OnPlatform.Platforms>
                </OnPlatform>
            </Label.FontFamily>
        </Label>
    </Grid>
</DataTemplate>
```

## FlyoutItem tab order

By default, the tab order of `FlyoutItem` objects is the same order in which they are listed in XAML, or programmatically added to a child collection. This order is the order in which the `FlyoutItem` objects will be navigated through with a keyboard, and often this default order is the best order.

The default tab order can be changed by setting the `FlyoutItem.TabIndex` property, which indicates the order in which `FlyoutItem` objects receive focus when the user navigates through items by pressing the Tab key. The default value of the property is 0, and it can be set to any `int` value.

The following rules apply when using the default tab order, or setting the `TabIndex` property:

- `FlyoutItem` objects with a `TabIndex` equal to 0 are added to the tab order based on their declaration order in XAML or child collections.
- `FlyoutItem` objects with a `TabIndex` greater than 0 are added to the tab order based on their `TabIndex` value.
- `FlyoutItem` objects with a `TabIndex` less than 0 are added to the tab order and appear before any zero value.
- Conflicts on a `TabIndex` are resolved by declaration order.

After defining a tab order, pressing the Tab key will cycle the focus through `FlyoutItem` objects in ascending `TabIndex` order, wrapping around to the beginning once the final object is reached.

In addition to setting the tab order of `FlyoutItem` objects, it may be necessary to exclude some objects from the tab order. This can be achieved with the `FlyoutItem.IsTabStop` property, which indicates whether a `FlyoutItem` is included in tab navigation. Its default value is `true`, and when its value is `false` the `FlyoutItem` is ignored by the tab-navigation infrastructure, irrespective if a `TabIndex` is set.

## Set the current FlyoutItem

The `Shell` class has a bindable property named `CurrentItem`, of type `FlyoutItem`, that represents the currently selected `FlyoutItem`. When a Shell application is first run, this property will be set to the first `FlyoutItem` in the subclassed `Shell` object. However, the property can be set to another `FlyoutItem`, as shown in the following example:

```xaml
<Shell ...
       CurrentItem="{x:Reference aboutItem}">
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        ...
    </FlyoutItem>
    <ShellContent x:Name="aboutItem"
                  Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />
</Shell>
```

This code sets the `ShellContent` object named `aboutItem` as the `CurrentItem` property, resulting in it being displayed. In this example, an implicit conversion is used to wrap the `ShellContent` object in a `Tab` object, which is wrapped in a `FlyoutItem` object.

The equivalent C# code is:

```csharp
Shell.Current.CurrentItem = aboutItem;
```

## Menu items

Menu items can be optionally added to the flyout, and each menu item is represented by a [`MenuItem`](xref:Xamarin.Forms.MenuItem) object. The position of `MenuItem` objects on the flyout is dependent upon their declaration order in the Shell visual hierarchy. Therefore, any `MenuItem` objects declared before `FlyoutItem` objects will appear at the top of the flyout, and any `MenuItem` objects declared after `FlyoutItem` objects will appear at the bottom of the flyout.

> [!NOTE]
> The `MenuItem` class has a [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) event, and a [`Command`](xref:Xamarin.Forms.MenuItem.Command) property. Therefore, `MenuItem` objects enable scenarios that execute an action in response to the `MenuItem` being tapped. These scenarios include performing navigation, and opening a web browser on a specific web page.

[`MenuItem`](xref:Xamarin.Forms.MenuItem) objects can be added to the flyout as shown in the following example:

```xaml
<Shell ...>
    ...            
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />    
</Shell>
```

This code adds two [`MenuItem`](xref:Xamarin.Forms.MenuItem) objects to the flyout, beneath all flyout items:

[![Screenshot of flyout containing MenuItem objects, on iOS and Android](flyout-images/flyout.png "Shell flyout containing MenuItem objects")](flyout-images/flyout-large.png#lightbox "Shell flyout containing MenuItem objects")

The first [`MenuItem`](xref:Xamarin.Forms.MenuItem) object executes an `ICommand` named `RandomPageCommand`, which navigates to a random page in the application. The second `MenuItem` object executes an `ICommand` named `HelpCommand`, which opens the URL specified by the `CommandParameter` property in a web browser.

> [!NOTE]
> The [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) of each `MenuItem` is inherited from the subclassed `Shell` object.

## Define MenuItem appearance

The appearance of each `MenuItem` can be customized by setting the `Shell.MenuItemTemplate` attached property to a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding Icon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Text}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.MenuItemTemplate>
    ...
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />  
</Shell>
```

This example attaches the Shell-level `MenuItemTemplate` to each `MenuItem` object, displaying the title of each `MenuItem` object in italics:

[![Screenshot of templated MenuItem objects, on iOS and Android](flyout-images/menuitem-templated.png "Shell templated MenuItem objects")](flyout-images/menuitem-templated-large.png#lightbox "Shell templated MenuItem objects")

> [!NOTE]
> Shell provides the [`Text`](xref:Xamarin.Forms.MenuItem.Text) and [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) properties to the [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) of the `MenuItemTemplate`. You can also use `Title` in place of `Text` and `Icon` in place of `IconImageSource` which will let you reuse the same template for Menu Items and Flyout Items

Because `Shell.MenuItemTemplate` is an attached property, different templates can be attached to specific `MenuItem` objects:

```xaml
<Shell ...>
    <Shell.MenuItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding Icon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Text}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.MenuItemTemplate>
    ...
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              Icon="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell">
        <Shell.MenuItemTemplate>
            <DataTemplate>
                ...
            </DataTemplate>
        </Shell.MenuItemTemplate>
    </MenuItem>
</Shell>
```


> [!NOTE]
> The same template used for [Flyout Items](#default-template-for-flyoutitems-and-menuitems) can also be used for Menu Items.

This example attaches the Shell-level `MenuItemTemplate` to the first `MenuItem` object, and attaches the inline `MenuItemTemplate` to the second `MenuItem`.

## Related links

- [Xaminals (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
