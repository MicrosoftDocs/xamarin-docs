---
title: "Hierarchical Navigation"
description: "This article demonstrates how to use the NavigationPage class to perform navigation in a stack of last-in, first-out (LIFO) pages."
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
---

# Hierarchical Navigation

_The NavigationPage class provides a hierarchical navigation experience where the user is able to navigate through pages, forwards and backwards, as desired. The class implements navigation as a last-in, first-out (LIFO) stack of Page objects. This article demonstrates how to use the NavigationPage class to perform navigation in a stack of pages._

This article discusses the following topics:

- [Performing navigation](#Performing_Navigation) – creating the root page, pushing pages to the navigation stack, popping pages from the navigation stack, and animating page transitions.
- [Passing data when navigating](#Passing_Data_when_Navigating) – passing data through a page constructor, and through a `BindingContext`.
- [Manipulating the navigation stack](#Manipulating_the_Navigation_Stack) – manipulating the stack by inserting or removing pages.

## Overview

To move from one page to another, an application will push a new page onto the navigation stack, where it will become the active page, as shown in the following diagram:

![](hierarchical-images/pushing.png "Pushing a Page to the Navigation Stack")

To return back to the previous page, the application will pop the current page from the navigation stack, and the new topmost page becomes the active page, as shown in the following diagram:

![](hierarchical-images/popping.png "Popping a Page from the Navigation Stack")

Navigation methods are exposed by the [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) property on any [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) derived types. These methods provide the ability to push pages onto the navigation stack, to pop pages from the navigation stack, and to perform stack manipulation.

<a name="Performing_Navigation" />

## Performing Navigation

In hierarchical navigation, the [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) class is used to navigate through a stack of [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) objects. The following screenshots show the main components of the `NavigationPage` on each platform:

![](hierarchical-images/navigationpage-components.png "NavigationPage Components")

The layout of a [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) is dependent on the platform:

- On iOS, a navigation bar is present at the top of the page that displays a title, and that has a *Back* button that returns to the previous page.
- On Android, a navigation bar is present at the top of the page that displays a title, an icon, and a *Back* button that returns to the previous page. The icon is defined in the `[Activity]` attribute that decorates the `MainActivity` class in the Android platform-specific project.
- On the Universal Windows Platform, a navigation bar is present at the top of the page that displays a title.

On all the platforms, the value of the [`Page.Title`](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) property will be displayed as the page title.

> [!NOTE]
> It's recommended that a `NavigationPage` should be populated with `ContentPage` instances only.

### Creating the Root Page

The first page added to a navigation stack is referred to as the *root* page of the application, and the following code example shows how this is accomplished:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

This causes the `Page1Xaml` [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) instance to be pushed onto the navigation stack, where it becomes the active page and the root page of the application. This is shown in the following screenshots:

![](hierarchical-images/mainpage.png "Root Page of Navigation Stack")

> [!NOTE]
> The [`RootPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.RootPage/) property of a [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance provides access to the first page in the navigation stack.

### Pushing Pages to the Navigation Stack

To navigate to `Page2Xaml`, it is necessary to invoke the [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) method on the [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) property of the current page, as demonstrated in the following code example:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

This causes the `Page2Xaml` instance to be pushed onto the navigation stack, where it becomes the active page. This is shown in the following screenshots:

![](hierarchical-images/secondpage.png "Page Pushed onto Navigation Stack")

When the [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) method is invoked, the following events occur:

- The page calling `PushAsync` has its [`OnDisappearing`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) override invoked.
- The page being navigated to has its [`OnAppearing`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) override invoked.
- The `PushAsync` task completes.

However, the precise order in which these events occur is platform dependent. For more information, see [Chapter 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) of Charles Petzold's Xamarin.Forms book.

> [!NOTE]
> Calls to the [`OnDisappearing`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) and [`OnAppearing`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) overrides cannot be treated as guaranteed indications of page navigation. For example, on iOS, the `OnDisappearing` override is called on the active page when the application terminates.

### Popping Pages from the Navigation Stack

The active page can be popped from the navigation stack by pressing the *Back* button on the device, regardless of whether this is a physical button on the device or an on-screen button.

To programmatically return to the original page, the `Page2Xaml` instance must invoke the [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) method, as demonstrated in the following code example:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

This causes the `Page2Xaml` instance to be removed from the navigation stack, with the new topmost page becoming the active page. When the [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) method is invoked, the following events occur:

- The page calling `PopAsync` has its [`OnDisappearing`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) override invoked.
- The page being returned to has its [`OnAppearing`](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) override invoked.
- The `PopAsync` task returns.

However, the precise order in which these events occur is platform dependent. For more information see [Chapter 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) of Charles Petzold's Xamarin.Forms book.

As well as [`PushAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) and [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) methods, the [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) property of each page also provides a [`PopToRootAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopToRootAsync()/) method, which is shown in the following code example:

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

This method pops all but the root [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) off the navigation stack, therefore making the root page of the application the active page.

### Animating Page Transitions

The [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) property of each page also provides overridden push and pop methods that include a `boolean` parameter that controls whether to display a page animation during navigation, as shown in the following code example:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushAsync (new Page2Xaml (), false);
}

async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopAsync (false);
}

async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopToRootAsync (false);
}
```

Setting the `boolean` parameter to `false` disables the page-transition animation, while setting the parameter to `true` enables the page-transition animation, provided that it is supported by the underlying platform. However, the push and pop methods that lack this parameter enable the animation by default.

<a name="Passing_Data_when_Navigating" />

## Passing Data when Navigating

Sometimes it's necessary for a page to pass data to another page during navigation. Two techniques for accomplishing this are passing data through a page constructor, and by setting the new page's [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) to the data. Each will now be discussed in turn.

### Passing Data through a Page Constructor

The simplest technique for passing data to another page during navigation is through a page constructor parameter, which is shown in the following code example:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

This code creates a `MainPage` instance, passing in the current date and time in ISO8601 format, which is wrapped in a [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) instance.

The `MainPage` instance receives the data through a constructor parameter, as shown in the following code example:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

The data is then displayed on the page by setting the [`Label.Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) property, as shown in the following screenshots:

![](hierarchical-images/passing-data-constructor.png "Data Passed Through a Page Constructor")

### Passing Data through a BindingContext

An alternative approach for passing data to another page during navigation is by setting the new page's [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) to the data, as shown in the following code example:

```csharp
async void OnNavigateButtonClicked (object sender, EventArgs e)
{
  var contact = new Contact {
    Name = "Jane Doe",
    Age = 30,
    Occupation = "Developer",
    Country = "USA"
  };

  var secondPage = new SecondPage ();
  secondPage.BindingContext = contact;
  await Navigation.PushAsync (secondPage);
}
```

This code sets the [`BindingContext`](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) of the `SecondPage` instance to the `Contact` instance, and then navigates to the `SecondPage`.

The `SecondPage` then uses data binding to display the `Contact` instance data, as shown in the following XAML code example:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="PassingData.SecondPage"
             Title="Second Page">
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
            ...
            <Button x:Name="navigateButton" Text="Previous Page" Clicked="OnNavigateButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

The following code example shows how the data binding can be accomplished in C#:

```csharp
public class SecondPageCS : ContentPage
{
  public SecondPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var navigateButton = new Button { Text = "Previous Page" };
    navigateButton.Clicked += OnNavigateButtonClicked;

    Content = new StackLayout {
      HorizontalOptions = LayoutOptions.Center,
      VerticalOptions = LayoutOptions.Center,
      Children = {
        new StackLayout {
          Orientation = StackOrientation.Horizontal,
          Children = {
            new Label{ Text = "Name:", FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)), HorizontalOptions = LayoutOptions.FillAndExpand },
            nameLabel
          }
        },
        ...
        navigateButton
      }
    };
  }

  async void OnNavigateButtonClicked (object sender, EventArgs e)
  {
    await Navigation.PopAsync ();
  }
}
```

The data is then displayed on the page by a series of [`Label`](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) controls, as shown in the following screenshots:

![](hierarchical-images/passing-data-bindingcontext.png "Data Passed Through a BindingContext")

For more information about data binding, see [Data Binding Basics](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Manipulating_the_Navigation_Stack" />

## Manipulating the Navigation Stack

The [`Navigation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) property exposes a [`NavigationStack`](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) property from which the pages in the navigation stack can be obtained. While Xamarin.Forms maintains access to the navigation stack, the `Navigation` property provides the [`InsertPageBefore`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) and [`RemovePage`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) methods for manipulating the stack by inserting pages or removing them.

The [`InsertPageBefore`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) method inserts a specified page in the navigation stack before an existing specified page, as shown in the following diagram:

![](hierarchical-images/insert-page-before.png "Inserting a Page in the Navigation Stack")

The [`RemovePage`](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) method removes the specified page from the navigation stack, as shown in the following diagram:

![](hierarchical-images/remove-page.png "Removing a Page from the Navigation Stack")

These methods enable a custom navigation experience, such as replacing a login page with a new page, following a successful login. The following code example demonstrates this scenario:

```csharp
async void OnLoginButtonClicked (object sender, EventArgs e)
{
  ...
  var isValid = AreCredentialsCorrect (user);
  if (isValid) {
    App.IsUserLoggedIn = true;
    Navigation.InsertPageBefore (new MainPage (), this);
    await Navigation.PopAsync ();
  } else {
    // Login failed
  }
}

```

Provided that the user's credentials are correct, the `MainPage` instance is inserted into the navigation stack before the current page. The [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) method then removes the current page from the navigation stack, with the `MainPage` instance becoming the active page.

## Summary

This article demonstrated how to use the [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) class to perform navigation in a stack of pages. This class provides a hierarchical navigation experience where the user is able to navigate through pages, forwards and backwards, as desired. The class implements navigation as a last-in, first-out (LIFO) stack of [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) objects.


## Related Links

- [Page Navigation](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Hierarchical (sample)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (sample)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (sample)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [How to Create a Sign In Screen Flow in Xamarin.Forms (Xamarin University Video) Sample](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [How to Create a Sign In Screen Flow in Xamarin.Forms (Xamarin University Video)](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)
