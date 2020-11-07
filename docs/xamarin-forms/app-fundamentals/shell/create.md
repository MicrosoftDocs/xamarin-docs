---
title: "Create a Xamarin.Forms Shell Application"
description: "The process for creating a Xamarin.Forms Shell application is to create a XAML file that subclasses the Shell class, set the MainPage property of the application's App class to the subclassed Shell object, and then describe the visual hierarchy of the application in the subclassed Shell class."
ms.prod: xamarin
ms.assetid: 2A51D78F-6CD5-4BC4-A62E-11CEFA799987
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Create a Xamarin.Forms Shell Application

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

The process for creating a Xamarin.Forms Shell application is as follows:

1. Create a new Xamarin.Forms application, or load an existing application that you want to convert to a Shell application.
1. Add a XAML file to the shared code project, that subclasses the `Shell` class. For more information, see [Subclass the Shell class](#subclass-the-shell-class).
1. Set the [`MainPage`](xref:Xamarin.Forms.Application.MainPage) property of the application's `App` class to the subclassed `Shell` object. For more information, see [Bootstrap the Shell application](#bootstrap-the-shell-application).
1. Describe the visual hierarchy of the application in the subclassed `Shell` class. For more information, see [Describe the visual hierarchy of the application](#describe-the-visual-hierarchy-of-the-application).

## Subclass the Shell class

The first step in creating a Xamarin.Forms Shell application is to add a XAML file to the shared code project that subclasses the `Shell` class. This file can be named anything, but **AppShell** is recommended. The following code example shows a newly created **AppShell.xaml** file:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="Xaminals.AppShell">

</Shell>
```

The following example shows the code-behind file, **AppShell.xaml.cs**:

```csharp
using Xamarin.Forms;

namespace Xaminals
{
    public partial class AppShell : Shell
    {
        public AppShell()
        {
            InitializeComponent();
        }
    }
}
```

## Bootstrap the Shell application

After creating the XAML file that subclasses the `Shell` object, the [`MainPage`](xref:Xamarin.Forms.Application.MainPage) property of the `App` class should be set to the subclassed `Shell` object:

```csharp
namespace Xaminals
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
        ...
    }
}
```

In this example, the `AppShell` class is the XAML file that derives from the `Shell` class.

> [!NOTE]
> While a blank Shell application builds, running it will result in a `InvalidOperationException` being thrown.

## Describe the visual hierarchy of the application

The final step in creating a Xamarin.Forms Shell application is to describe the visual hierarchy of the application, in the subclassed `Shell` class. A subclassed `Shell` class consists of three main hierarchical objects:

- `FlyoutItem` or `TabBar`. A `FlyoutItem` represents one or more items in the flyout, and should be used when the navigation pattern for the application includes a flyout. A `TabBar` represents the bottom tab bar, and should be used when the navigation pattern for the application begins with bottom tabs. Every `FlyoutItem` object or `TabBar` object is a child of the `Shell` object.
- `Tab`, which represents grouped content, navigable by bottom tabs. Every `Tab` object is a child of a `FlyoutItem` object or a `TabBar` object.
- `ShellContent`, which represents the `ContentPage` objects in your application. Every `ShellContent` object is a child of a `Tab` object. When more than one `ShellContent` object is present in a `Tab`, the objects will be navigable by top tabs.

None of these objects represent any user interface, but rather the organization of the application's visual hierarchy. Shell will take these objects and produce the navigation user interface for the content.

The following XAML shows an example of a subclassed `Shell` class:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    ...
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png">
                <views:CatsPage />
            </ShellContent>
            <ShellContent Title="Dogs"
                          Icon="dog.png">
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png">
            <views:MonkeysPage />
        </ShellContent>
        <ShellContent Title="Elephants"
                      Icon="elephant.png">  
            <views:ElephantsPage />
        </ShellContent>
        <ShellContent Title="Bears"
                      Icon="bear.png">
            <views:BearsPage />
        </ShellContent>
    </FlyoutItem>
    ...
</Shell>
```

When run, this XAML displays the `CatsPage`, because it's the first item of content declared in the subclassed `Shell` class:

[![Screenshot of a Shell app, on iOS and Android](create-images/cats.png "Shell app")](create-images/cats-large.png#lightbox "Shell app")

Pressing the hamburger icon, or swiping from the left, displays the flyout:

[![Screenshot of a Shell flyout, on iOS and Android](create-images/flyout-reduced.png "Shell flyout")](create-images/flyout-reduced-large.png#lightbox "Shell flyout")

> [!IMPORTANT]
> In a Shell application, each [`ContentPage`](xref:Xamarin.Forms.ContentPage) that's a child of a `ShellContent` object is created during application startup. Adding additional `ShellContent` objects using this approach will result in additional pages being created during application startup, which can lead to a poor startup experience. However, Shell is also capable of creating pages on demand, in response to navigation. For more information, see [Efficient page loading](tabs.md#efficient-page-loading) in the [Xamarin.Forms Shell Tabs](tabs.md) guide.

## Related links

- [Xaminals (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)