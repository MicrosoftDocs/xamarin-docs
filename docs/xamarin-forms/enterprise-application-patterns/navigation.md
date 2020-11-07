---
title: "Enterprise App Navigation"
description: "This chapter explains how the eShopOnContainers mobile app performs view model-first navigation from view models."
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Enterprise App Navigation

> [!NOTE]
> This eBook was published in the spring of 2017, and has not been updated since then. There is much in the book that remains valuable, but some of the material is outdated.

Xamarin.Forms includes support for page navigation, which typically results from the user's interaction with the UI or from the app itself as a result of internal logic-driven state changes. However, navigation can be complex to implement in apps that use the Model-View-ViewModel (MVVM) pattern, as the following challenges must be met:

- How to identify the view to be navigated to, using an approach that does not introduce tight coupling and dependencies between views.
- How to coordinate the process by which the view to be navigated to is instantiated and initialized. When using MVVM, the view and view model need to be instantiated and associated with each other via the view's binding context. When an app is using a dependency injection container, the instantiation of views and view models might require a specific construction mechanism.
- Whether to perform view-first navigation, or view model-first navigation. With view-first navigation, the page to navigate to refers to the name of the view type. During navigation, the specified view is instantiated, along with its corresponding view model and other dependent services. An alternative approach is to use view model-first navigation, where the page to navigate to refers to the name of the view model type.
- How to cleanly separate the navigational behavior of the app across the views and view models. The MVVM pattern provides a separation between the app's UI and its presentation and business logic. However, the navigation behavior of an app will often span the UI and presentations parts of the app. The user will often initiate navigation from a view, and the view will be replaced as a result of the navigation. However, navigation might often also need to be initiated or coordinated from within the view model.
- How to pass parameters during navigation for initialization purposes. For example, if the user navigates to a view to update order details, the order data will have to be passed to the view so that it can display the correct data.
- How to co-ordinate navigation, to ensure that certain business rules are obeyed. For example, users might be prompted before navigating away from a view so that they can correct any invalid data or be prompted to submit or discard any data changes that were made within the view.

This chapter addresses these challenges by presenting a `NavigationService` class that's used to perform view model-first page navigation.

> [!NOTE]
> The `NavigationService` used by the app is designed only to perform hierarchical navigation between ContentPage instances. Using the service to navigate between other page types might result in unexpected behavior.

## Navigating Between Pages

Navigation logic can reside in a view's code-behind, or in a data bound view model. While placing navigation logic in a view might be the simplest approach, it is not easily testable through unit tests. Placing navigation logic in view model classes means that the logic can be exercised through unit tests. In addition, the view model can then implement logic to control navigation to ensure that certain business rules are enforced. For example, an app might not allow the user to navigate away from a page without first ensuring that the entered data is valid.

A `NavigationService` class is typically invoked from view models, to promote testability. However, navigating to views from view models would require the view models to reference views, and particularly views that the active view model isn't associated with, which is not recommended. Therefore, the `NavigationService` presented here specifies the view model type as the target to navigate to.

The eShopOnContainers mobile app uses the `NavigationService` class to provide view model-first navigation. This class implements the `INavigationService` interface, which is shown in the following code example:

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

This interface specifies that an implementing class must provide the following methods:

|Method|Purpose|
|--- |--- |
|`InitializeAsync`|Performs navigation to one of two pages when the app is launched.|
|`NavigateToAsync`|Performs hierarchical navigation to a specified page.|
|`NavigateToAsync(parameter)`|Performs hierarchical navigation to a specified page, passing a parameter.|
|`RemoveLastFromBackStackAsync`|Removes the previous page from the navigation stack.|
|`RemoveBackStackAsync`|Removes all the previous pages from the navigation stack.|

In addition, the `INavigationService` interface specifies that an implementing class must provide a `PreviousPageViewModel` property. This property returns the view model type associated with the previous page in the navigation stack.

> [!NOTE]
> An `INavigationService` interface would usually also specify a `GoBackAsync` method, which is used to programmatically return to the previous page in the navigation stack. However, this method is missing from the eShopOnContainers mobile app because it's not required.

### Creating the NavigationService Instance

The `NavigationService` class, which implements the `INavigationService` interface, is registered as a singleton with the Autofac dependency injection container, as demonstrated in the following code example:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

The `INavigationService` interface is resolved in the `ViewModelBase` class constructor, as demonstrated in the following code example:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

This returns a reference to the `NavigationService` object that's stored in the Autofac dependency injection container, which is created by the `InitNavigation` method in the `App` class. For more information, see [Navigating When the App is Launched](#navigating-when-the-app-is-launched).

The `ViewModelBase` class stores the `NavigationService` instance in a `NavigationService` property, of type `INavigationService`. Therefore, all view model classes, which derive from the `ViewModelBase` class, can use the `NavigationService` property to access the methods specified by the `INavigationService` interface. This avoids the overhead of injecting the `NavigationService` object from the Autofac dependency injection container into each view model class.

### Handling Navigation Requests

Xamarin.Forms provides the [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) class, which implements a hierarchical navigation experience in which the user is able to navigate through pages, forwards and backwards, as desired. For more information about hierarchical navigation, see [Hierarchical Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

Rather than use the [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) class directly, the eShopOnContainers app wraps the `NavigationPage` class in the `CustomNavigationView` class, as shown in the following code example:

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

The purpose of this wrapping is for ease of styling the [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) instance inside the XAML file for the class.

Navigation is performed inside view model classes by invoking one of the `NavigateToAsync` methods, specifying the view model type for the page being navigated to, as demonstrated in the following code example:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

The following code example shows the `NavigateToAsync` methods provided by the `NavigationService` class:

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

Each method allows any view model class that derives from the `ViewModelBase` class to perform hierarchical navigation by invoking the `InternalNavigateToAsync` method. In addition, the second `NavigateToAsync` method enables navigation data to be specified as an argument that's passed to the view model being navigated to, where it's typically used to perform initialization. For more information, see [Passing Parameters During Navigation](#passing-parameters-during-navigation).

The `InternalNavigateToAsync` method executes the navigation request, and is shown in the following code example:

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

The `InternalNavigateToAsync` method performs navigation to a view model by first calling the `CreatePage` method. This method locates the view that corresponds to the specified view model type, and creates and returns an instance of this view type. Locating the view that corresponds to the view model type uses a convention-based approach, which assumes that:

- Views are in the same assembly as view model types.
- Views are in a .Views child namespace.
- View models are in a .ViewModels child namespace.
- View names correspond to view model names, with "Model" removed.

When a view is instantiated, it's associated with its corresponding view model. For more information about how this occurs, see [Automatically Creating a View Model with a View Model Locator](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically-creating-a-view-model-with-a-view-model-locator).

If the view being created is a `LoginView`, it's wrapped inside a new instance of the `CustomNavigationView` class and assigned to the [`Application.Current.MainPage`](xref:Xamarin.Forms.Application.MainPage) property. Otherwise, the `CustomNavigationView` instance is retrieved, and provided that it isn't null, the [`PushAsync`](xref:Xamarin.Forms.NavigationPage) method is invoked to push the view being created onto the navigation stack. However, If the retrieved `CustomNavigationView` instance is `null`, the view being created is wrapped inside a new instance of the `CustomNavigationView` class and assigned to the `Application.Current.MainPage` property. This mechanism ensures that during navigation, pages are added correctly to the navigation stack both when it's empty, and when it contains data.

> [!TIP]
> Consider caching pages. Page caching results in memory consumption for views that are not currently displayed. However, without page caching it does mean that XAML parsing and construction of the page and its view model will occur every time a new page is navigated to, which can have a performance impact for a complex page. For a well-designed page that does not use an excessive number of controls, the performance should be sufficient. However, page caching might help if slow page loading times are encountered.

After the view is created and navigated to, the `InitializeAsync` method of the view's associated view model is executed. For more information, see [Passing Parameters During Navigation](#passing-parameters-during-navigation).

### Navigating When the App is Launched

When the app is launched, the `InitNavigation` method in the `App` class is invoked. The following code example shows this method:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

The method creates a new `NavigationService` object in the Autofac dependency injection container, and returns a reference to it, before invoking its `InitializeAsync` method.

> [!NOTE]
> When the `INavigationService` interface is resolved by the `ViewModelBase` class, the container returns a reference to the `NavigationService` object that was created when the InitNavigation method is invoked.

The following code example shows the `NavigationService` `InitializeAsync` method:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

The `MainView` is navigated to if the app has a cached access token, which is used for authentication. Otherwise, the `LoginView` is navigated to.

For more information about the Autofac dependency injection container, see [Introduction to Dependency Injection](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction-to-dependency-injection).

### Passing Parameters During Navigation

One of the `NavigateToAsync` methods, specified by the `INavigationService` interface, enables navigation data to be specified as an argument that's passed to the view model being navigated to, where it's typically used to perform initialization.

For example, the `ProfileViewModel` class contains an `OrderDetailCommand` that's executed when the user selects an order on the `ProfileView` page. In turn, this executes the `OrderDetailAsync` method, which is shown in the following code example:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

This method invokes navigation to the `OrderDetailViewModel`, passing an `Order` instance that represents the order that the user selected on the `ProfileView` page. When the `NavigationService` class creates the `OrderDetailView`, the `OrderDetailViewModel` class is instantiated and assigned to the view's [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext). After navigating to the `OrderDetailView`, the `InternalNavigateToAsync` method executes the `InitializeAsync` method of the view's associated view model.

The `InitializeAsync` method is defined in the `ViewModelBase` class as a method that can be overridden. This method specifies an `object` argument that represents the data to be passed to a view model during a navigation operation. Therefore, view model classes that want to receive data from a navigation operation provide their own implementation of the `InitializeAsync` method to perform the required initialization. The following code example shows the `InitializeAsync` method from the `OrderDetailViewModel` class:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

This method retrieves the `Order` instance that was passed into the view model during the navigation operation, and uses it to retrieve the full order details from the `OrderService` instance.

### Invoking Navigation using Behaviors

Navigation is usually triggered from a view by a user interaction. For example, the `LoginView` performs navigation following successful authentication. The following code example shows how the navigation is invoked by a behavior:

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

At runtime, the `EventToCommandBehavior` will respond to interaction with the [`WebView`](xref:Xamarin.Forms.WebView). When the `WebView` navigates to a web page, the [`Navigating`](xref:Xamarin.Forms.WebView.Navigating) event will fire, which will execute the `NavigateCommand` in the `LoginViewModel`. By default, the event arguments for the event are passed to the command. This data is converted as it's passed between source and target by the converter specified in the `EventArgsConverter` property, which returns the [`Url`](xref:Xamarin.Forms.WebNavigationEventArgs.Url) from the [`WebNavigatingEventArgs`](xref:Xamarin.Forms.WebNavigatingEventArgs). Therefore, when the `NavigationCommand` is executed, the Url of the web page is passed as a parameter to the registered `Action`.

In turn, the `NavigationCommand` executes the `NavigateAsync` method, which is shown in the following code example:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

This method invokes navigation to the `MainViewModel`, and following navigation, removes the `LoginView` page from the navigation stack.

### Confirming or Cancelling Navigation

An app might need to interact with the user during a navigation operation, so that the user can confirm or cancel navigation. This might be necessary, for example, when the user attempts to navigate before having fully completed a data entry page. In this situation, an app should provide a notification that allows the user to navigate away from the page, or to cancel the navigation operation before it occurs. This can be achieved in a view model class by using the response from a notification to control whether or not navigation is invoked.

## Summary

Xamarin.Forms includes support for page navigation, which typically results from the user's interaction with the UI, or from the app itself, as a result of internal logic-driven state changes. However, navigation can be complex to implement in apps that use the MVVM pattern.

This chapter presented a `NavigationService` class, which is used to perform view model-first navigation from view models. Placing navigation logic in view model classes means that the logic can be exercised through automated tests. In addition, the view model can then implement logic to control navigation to ensure that certain business rules are enforced.

## Related Links

- [Download eBook (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
