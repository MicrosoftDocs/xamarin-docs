---
title: "Xamarin.Forms Shell Layout"
description: "After a flyout, the next level of navigation in a Shell application is the bottom tab bar. Alternatively, the navigation pattern for an application can begin with bottom tabs and make no use of a flyout. In both cases, when a bottom tab contains more than one page, the pages will be navigable by top tabs."
ms.prod: xamarin
ms.assetid: 318D81DB-E456-4E44-B083-36A27DBD9523
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/06/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shell Tabs

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

When the navigation pattern for an application includes a flyout, the next level of navigation in the application is the bottom tab bar. In addition, when the flyout is closed the bottom tab bar can be considered to be the top level of navigation.

Alternatively, the navigation pattern for an application can begin with bottom tabs and make no use of a flyout. In this scenario, the child of the `Shell` object should be a `TabBar` object, which represents the bottom tab bar.

> [!NOTE]
> The `TabBar` type disables the flyout.

Each `FlyoutItem` or `TabBar` object can contain one or more `Tab` objects, with each `Tab` object representing a tab on the bottom tab bar. Each `Tab` object can contain one or more `ShellContent` objects, and each `ShellContent` object will display a single [`ContentPage`](xref:Xamarin.Forms.ContentPage) object. When more than one `ShellContent` object is present in a `Tab` object, the `ContentPage` objects will be navigable by top tabs.

Within each [`ContentPage`](xref:Xamarin.Forms.ContentPage) object, additional `ContentPage` objects can be navigated to. For more information about navigation, see [Xamarin.Forms Shell Navigation](navigation.md).

## Single page application

The simplest Shell application is a single page application, which can be created by adding a single `Tab` object to a `TabBar` object. Within the `Tab` object, a `ShellContent` object should be set to a [`ContentPage`](xref:Xamarin.Forms.ContentPage) object:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

This code example results in the following single page application:

[![Screenshot of a Shell single page app, on iOS and Android](tabs-images/single-page-app.png "Shell single page app")](tabs-images/single-page-app-large.png#lightbox "Shell single page app")

> [!NOTE]
> The navigation bar can be hidden, if required, by setting the `Shell.NavBarIsVisible` attached property to `false` on the [`ContentPage`](xref:Xamarin.Forms.ContentPage) object.

Shell has implicit conversion operators that enable the Shell visual hierarchy to be simplified, without introducing additional views into the visual tree. This is possible because a subclassed `Shell` object can only ever contain `FlyoutItem` objects or a `TabBar` object, which can only ever contain `Tab` objects, which can only ever contain `ShellContent` objects. These implicit conversion operators can be used to remove the `TabBar`, `Tab`, and `ShellContent` objects from the previous example:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <views:CatsPage />
</Shell>
```

This implicit conversion automatically wraps the [`ContentPage`](xref:Xamarin.Forms.ContentPage) object in a `ShellContent` object, which is wrapped in a `Tab` object, which is wrapped in a `FlyoutItem` object. A flyout is unnecessary in a single page application, and therefore the `Shell.FlyoutBehavior` property is set to `Disabled`.

> [!IMPORTANT]
> In a Shell application, each [`ContentPage`](xref:Xamarin.Forms.ContentPage) that's a child of a `ShellContent` object is created during application startup. Adding additional `ShellContent` objects using this approach will result in additional pages being created during application startup, which can lead to a poor startup experience. However, Shell is also capable of creating pages on demand, in response to navigation. For more information, see [Efficient page loading](tabs.md#efficient-page-loading).

## Bottom tabs

`Tab` objects are rendered as bottom tabs, provided that there are multiple `Tab` objects in a single `TabBar` object:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

Tab titles and icons are set on each `Tab` object, and displayed on the bottom tabs:

[![Screenshot of a Shell two page app with bottom tabs, on iOS and Android](tabs-images/two-page-app-bottom-tabs.png "Shell two page app with bottom tabs")](tabs-images/two-page-app-bottom-tabs-large.png#lightbox "Shell two page app with bottom tabs")

When there are more than five tabs, a **More** tab will appear, which can be used to access the additional tabs:

[![Screenshot of a Shell app with a More tab, on iOS and Android](tabs-images/more-tabs.png "Shell app with More tab")](tabs-images/more-tabs-large.png#lightbox "Shellapp with More tabs")

Alternatively, Shell's implicit conversion operators can be used to remove the `ShellContent` and `Tab` objects from the previous example:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <views:CatsPage IconImageSource="cat.png" />
        <views:DogsPage IconImageSource="dog.png" />
    </TabBar>
</Shell>
```

This implicit conversion automatically wraps each [`ContentPage`](xref:Xamarin.Forms.ContentPage) object in a `ShellContent` object, which are then both wrapped in a `Tab` object.

> [!IMPORTANT]
> In a Shell application, each [`ContentPage`](xref:Xamarin.Forms.ContentPage) that's a child of a `ShellContent` object is created during application startup. Adding additional `ShellContent` objects using this approach will result in additional pages being created during application startup, which can lead to a poor startup experience. However, Shell is also capable of creating pages on demand, in response to navigation. For more information, see [Efficient page loading](tabs.md#efficient-page-loading).

### Tab class

The `Tab` class includes the following properties that control tab appearance and behavior:

- `CurrentItem`, of type `ShellContent`, the selected item.
- `FlyoutDisplayOptions`, of type `FlyoutDisplayOptions`, defines how the item and its children are displayed in the flyout. The default value is `AsSingleItem`.
- `FlyoutIcon`, of type `ImageSource`, defines the icon that will be displayed in the flyout.
- `Icon`, of type `ImageSource`, defines the icon to display in parts of the chrome which are not the flyout.
- `IsChecked`, of type `boolean`, defines if the item is currently highlighted in the flyout.
- `IsEnabled`, of type `boolean`, defines if the item is selectable in the chrome.
- `IsTabStop`, of type `bool`, indicates whether a `Tab` is included in tab navigation. Its default value is `true`, and when its value is `false` the `Tab` is ignored by the tab-navigation infrastructure, irrespective if a `TabIndex` is set.
- `Items`, of type `IList<ShellContent>`, defines all the content within a `Tab`.
- `TabIndex`, of type `int`, indicates the order in which `Tab` objects receive focus when the user navigates through items by pressing the Tab key. The default value of the property is 0.
- `Title`, of type `string`, the title to display on the tab in the UI.

## Shell content

The child of every `Tab` object is a `ShellContent` object, whose `Content` property is set to a [`ContentPage`](xref:Xamarin.Forms.ContentPage):

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

Within each [`ContentPage`](xref:Xamarin.Forms.ContentPage) object, additional `ContentPage` objects can be navigated to. For more information about navigation, see [Xamarin.Forms Shell Navigation](navigation.md).

> [!NOTE]
> The [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) of each `ShellContent` object is inherited from the parent `Tab` object.

### ShellContent class

The `ShellContent` class includes the following properties that control tab content appearance and behavior:

- `Content`, of type `object`, the content of the `ShellContent`.
- `ContentTemplate`, of type `DataTemplate`, the template used to dynamically inflate the content of the `ShellContent`.
- `FlyoutIcon`, of type `ImageSource`, defines the icon that will be displayed in the flyout.
- `Icon`, of type `ImageSource`, defines the icon to display in parts of the chrome which are not the flyout.
- `IsChecked`, of type `boolean`, defines if the item is currently highlighted in the flyout.
- `IsEnabled`, of type `boolean`, defines if the item is selectable in the chrome.
- `IsVisible`, of type `bool`, indicates if the `ShellContent` is hidden from all UI structures. Its default value is `true`.
- `MenuItems`, of type `MenuItemCollection`, the menu items to display in the flyout when this `ShellContent` is the presented page.
- `Title`, of type `string`, the title to display in the UI.

All of these properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that the properties can be targets of data bindings.

## Bottom and top tabs

When more than one `ShellContent` object is present in a `Tab` object, a top tab bar is added to the bottom tab, through which the [`ContentPage`](xref:Xamarin.Forms.ContentPage) objects are navigable:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Monkeys"
             Icon="monkey.png">
            <ShellContent>
                <views:MonkeysPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

This results in the layout shown in the following screenshots:

[![Screenshot of a Shell two page app with top and bottom tabs, on iOS and Android](tabs-images/two-page-app-top-tabs.png "Shell two page app with top and bottom tabs")](tabs-images/two-page-app-top-tabs-large.png#lightbox "Shell two page app with top and bottom tabs")

Alternatively, Shell's implicit conversion operators can be used to remove the `ShellContent` objects, and the second `Tab` object from the previous example:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <views:CatsPage />
            <views:DogsPage />
        </Tab>
        <views:MonkeysPage IconImageSource="monkey.png" />
    </TabBar>
</Shell>
```

This implicit conversion automatically wraps `MonkeysPage` in a `ShellContent` object, which is then wrapped in a `Tab` object. In addition, `CatsPage` and `DogsPage` are implicitly wrapped in `ShellContent` objects.

## Efficient page loading

In a Shell application, every [`ContentPage`](xref:Xamarin.Forms.ContentPage) object in a `ShellContent` object is created during application startup, which can lead to a poor startup experience. However, Shell also allows pages to be created on demand, in response to navigation. This can be accomplished by using the `DataTemplate` markup extension to convert each `ContentPage` into a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), and then setting the result as the `ShellContent.ContentTemplate` property value:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">    
    <TabBar>
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
    </TabBar>    
</Shell>
```

This XAML creates and displays `CatsPage`, because it's the first item of content declared in the subclassed `Shell` object. `DogsPage` and `MonkeysPage` can be navigated to via bottom tabs, and these pages are only created when the user navigates to them. The advantage of this approach is that the poor startup experience is avoided, as pages are created on demand in response to navigation, rather than at application startup.

## Tab appearance

The `Shell` class defines the following attached properties that control the appearance of tabs:

- `TabBarBackgroundColor`, of type `Color`, that defines the background color for the tab bar. If the property is unset, the `BackgroundColor` property value is used.
- `TabBarDisabledColor`, of type `Color`, that defines the disabled color for the tab bar. If the property is unset, the `DisabledColor` property value is used.
- `TabBarForegroundColor`, of type `Color`, that defines the foreground color for the tab bar. If the property is unset, the `ForegroundColor` property value is used.
- `TabBarTitleColor`, of type `Color`, that defines the title color for the tab bar. If the property is unset, the `TitleColor` property value will be used.
- `TabBarUnselectedColor`, of type `Color`, that defines the unselected color for the tab bar. If the property is unset, the `UnselectedColor` property value is used.

All of these properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that the properties can be targets of data bindings, and styled.

The following example shows a XAML style that sets different tab color properties:

```xaml
<Style x:Key="BaseStyle"
       TargetType="Element">
    <Setter Property="Shell.TabBarBackgroundColor"
            Value="#3498DB" />
    <Setter Property="Shell.TabBarTitleColor"
            Value="White" />
    <Setter Property="Shell.TabBarUnselectedColor"
            Value="#B4FFFFFF" />
</Style>
```

In addition, tabs can also be styled using Cascading Style Sheets (CSS). For more information, see [Xamarin.Forms Shell specific properties](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## Related links

- [Xaminals (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Xamarin.Forms Shell Navigation](navigation.md)
- [Xamarin.Forms CSS Shell specific properties](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)