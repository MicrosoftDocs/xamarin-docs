---
title: "Xamarin.Forms Shell Navigation"
description: "Xamarin.Forms Shell applications can utilize a URI-based navigation experience that permits navigation to any page in the application, without having to follow a set navigation hierarchy."
ms.prod: xamarin
ms.assetid: 57079D89-D1CB-48BD-9FEE-539CEC29EABB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/06/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shell Navigation

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms Shell includes a URI-based navigation experience that uses routes to navigate to any page in the application, without having to follow a set navigation hierarchy. In addition, it also provides the ability to navigate backwards without having to visit all of the pages on the navigation stack.

`Shell` defines the following navigation related properties:

- `BackButtonBehavior`, of type `BackButtonBehavior`, an attached property that defines the behavior of the back button.
- `CurrentItem`, of type `FlyoutItem`, the currently selected `FlyoutItem`.
- `CurrentPage`, of type `Page`, the currently presented page.
- `CurrentState`, of type `ShellNavigationState`, the current navigation state of the `Shell`.
- `Current`, of type `Shell`, a type-casted alias for `Application.Current.MainPage`.

The `BackButtonBehavior`, `CurrentItem`, and `CurrentState` properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that these properties can be targets of data bindings.

Navigation is performed by invoking the `GoToAsync` method, from the `Shell` class. When navigation is about to be performed, a `Navigating` event is fired, and a `Navigated` event is fired when navigation completes.

> [!NOTE]
> Navigation can still be performed in a Xamarin.Forms Shell application by using the [Navigation](xref:Xamarin.Forms.NavigableElement.Navigation) property. For more information, see [Hierarchical Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

## Routes

Navigation is performed in a Shell application by specifying a URI to navigate to. Navigation URIs can have three components:

- A *route*, which defines the path to content that exists as part of the Shell visual hierarchy.
- A *page*. Pages that don't exist in the Shell visual hierarchy can be pushed onto the navigation stack from anywhere within a Shell application. For example, an item details page won't be defined in the Shell visual hierarchy, but can be pushed onto the navigation stack as required.
- One or more *query parameters*. Query parameters are parameters that can be passed to the destination page while navigating.

When a navigation URI includes all three components, the structure is: //route/page?queryParameters

### Register routes

Routes can be defined on `FlyoutItem`, `Tab`, and `ShellContent` objects, through their `Route` properties:

```xaml
<Shell ...>
    <FlyoutItem ...
                Route="animals">
        <Tab ...
             Route="domestic">
            <ShellContent ...
                          Route="cats" />
            <ShellContent ...
                          Route="dogs" />
        </Tab>
        <ShellContent ...
                      Route="monkeys" />
        <ShellContent ...
                      Route="elephants" />  
        <ShellContent ...
                      Route="bears" />
    </FlyoutItem>
    <ShellContent ...
                  Route="about" />                  
    ...
</Shell>
```

> [!NOTE]
> All items in the Shell hierarchy have a route associated with them. If a route is not set by the developer, the route is generated at runtime. However, generated routes are not guaranteed to be consistent across different application sessions.

This example, from the sample application, creates the following route hierarchy, which can be used in programmatic navigation:

```
animals
  domestic
    cats
    dogs
  monkeys
  elephants
  bears
about
```

To navigate to the `ShellContent` object for the `dogs` route, the absolute route URI is `//animals/domestic/dogs`. Similarly, to navigate to the `ShellContent` object for the `about` route, the absolute route URI is `//about`.

> [!IMPORTANT]
> An `ArgumentException` will be thrown on application startup if a duplicate route is detected. This exception will also be thrown if two or more routes at the same level in the hierarchy share a route name.

#### Register page routes

In the Shell subclass constructor, or any other location that runs before a route is invoked, additional routes can be explicitly registered for any pages that aren't represented in the Shell visual hierarchy:

```csharp
Routing.RegisterRoute("monkeydetails", typeof(MonkeyDetailPage));
Routing.RegisterRoute("beardetails", typeof(BearDetailPage));
Routing.RegisterRoute("catdetails", typeof(CatDetailPage));
Routing.RegisterRoute("dogdetails", typeof(DogDetailPage));
Routing.RegisterRoute("elephantdetails", typeof(ElephantDetailPage));
```

This example registers item detail pages, that aren't defined in the Shell subclass, as routes. These pages can then be navigated to using URI-based navigation, from anywhere within the application. The routes for such pages are known as *global routes*.

> [!NOTE]
> Pages whose routes have been registered with the `Routing.RegisterRoute` method can be deregistered with the `Routing.UnRegisterRoute` method, if required.

Alternatively, pages can be registered at different route hierarchies if required:

```csharp
Routing.RegisterRoute("monkeys/details", typeof(MonkeyDetailPage));
Routing.RegisterRoute("bears/details", typeof(BearDetailPage));
Routing.RegisterRoute("cats/details", typeof(CatDetailPage));
Routing.RegisterRoute("dogs/details", typeof(DogDetailPage));
Routing.RegisterRoute("elephants/details", typeof(ElephantDetailPage));
```

This example enables contextual page navigation, where navigating to the `details` route from the page for the `monkeys` route displays the `MonkeyDetailPage`. Similarly, navigating to the `details` route from the page for the `elephants` route displays the `ElephantDetailPage`.

> [!IMPORTANT]
> An `ArgumentException` will be thrown if the `Routing.RegisterRoute` method attempts to register the same route to two or more different types.

## Perform navigation

To perform navigation, a reference to the `Shell` subclass must first be obtained. This reference can be obtained by casting the `App.Current.MainPage` property to a `Shell` object, or through the `Shell.Current` property. Navigation can then be performed by calling the `GoToAsync` method on the `Shell` object. This method navigates to a `ShellNavigationState` and returns a `Task` that will complete once the navigation animation has completed. The `ShellNavigationState` object is constructed by the `GoToAsync` method, from a `string`, or a `Uri`, and it has its `Location` property set to the `string` or `Uri` argument.

When a route from the Shell visual hierarchy is navigated to, a navigation stack isn't created. However, when a page that's not in the Shell visual hierarchy is navigated to, a navigation stack is created.

> [!NOTE]
> The current navigation state of the `Shell` can be retrieved through the `Shell.Current.CurrentState` property, which includes the URI of the displayed route in the `Location` property.

### Absolute routes

Navigation can be performed by specifying a valid absolute URI as an argument to the `GoToAsync` method:

```csharp
await Shell.Current.GoToAsync("//animals/monkeys");
```

This example navigates to the page for the `monkeys` route, with the route being defined on a `ShellContent` object. The `ShellContent` object that represents the `monkeys` route is a child of a `FlyoutItem` object, whose route is `animals`.

### Relative routes

Navigation can also be performed by specifying a valid relative URI as an argument to the `GoToAsync` method. The routing system will attempt to match the URI to a `ShellContent` object. Therefore, if all the routes in an application are unique, navigation can be performed by only specifying the unique route name as a relative URI.

The following relative route formats are supported:

| Format | Description |
| --- | --- |
| *route* | The route hierarchy will be searched for the specified route, upwards from from the current position. The matching page will be pushed to the navigation stack. |
| /*route* | The route hierarchy will be searched from the specified route, downwards from the current position. The matching page will be pushed to the navigation stack. |
| //*route* | The route hierarchy will be searched for the specified route, upwards from the current position. The matching page will replace the navigation stack. |
| ///*route* | The route hierarchy will be searched for the specified route, downwards from the current position. The matching page will replace the navigation stack. |

The following example navigates to the page for the `monkeydetails` route:

```csharp
await Shell.Current.GoToAsync("monkeydetails");
```

In this example, the `monkeyDetails` route is searched for up the hierarchy until the matching page is found. When the page is found, it's pushed to the navigation stack.

#### Contextual navigation

Relative routes enable contextual navigation. For example, consider the following route hierarchy:

```
monkeys
  details
bears
  details
```

When the registered page for the `monkeys` route is displayed, navigating to the `details` route will display the registered page for the `monkeys/details` route. Similarly, when the registered page for the `bears` route is displayed, navigating to the `details` route will display the registered page for the `bears/details` route. For information on how to register the routes in this example, see [Register page routes](#register-page-routes).

### Backwards navigation

Backwards navigation can be performed by specifying ".." as the argument to the `GotoAsync` method:

```csharp
await Shell.Current.GoToAsync("..");
```

Backwards navigation with ".." can also be combined with a route:

```csharp
await Shell.Current.GoToAsync("../route");
```

In this example, backwards navigation is performed, and then navigation to the specified route.

> [!IMPORTANT]
> Navigating backwards and into a specified route is only possible if the backwards navigation places you at the current location in the route hierarchy to navigate to the specified route.

Similarly, it's possible to navigate backwards multiple times, and then navigate to a specified route:

```csharp
await Shell.Current.GoToAsync("../../route");
```

In this example, backwards navigation is performed twice, and then navigation to the specified route.

In addition, data can be passed through query properties when navigating backwards:

```csharp
await Shell.Current.GoToAsync($"..?parameterToPassBack={parameterValueToPassBack}");
```

In this example, backwards navigation is performed, and the query parameter value is passed to the query parameter on the previous page.

> [!NOTE]
> Query parameters can be appended to any backwards navigation request.

For more information about passing data when navigating, see [Pass data](#pass-data).

### Invalid routes

The following route formats are invalid:

| Format | Explanation |
| --- | --- |
| *route* or /*route* | Routes in the visual hierarchy can't be pushed onto the navigation stack. |
| //*page* or ///*page* | Global routes currently can't be the only page on the navigation stack. Therefore, absolute routing to global routes is unsupported. |

Use of any of these route formats result in an `Exception` being thrown.

> [!IMPORTANT]
> Attempting to navigate to a non-existent route results in an `ArgumentException` exception being thrown.

### Debugging navigation

Some of the Shell classes are decorated with the `DebuggerDisplayAttribute`, which specifies how a class or field is displayed by the debugger. This can help to debug navigation requests by displaying data related to the navigation request. For example, the following screenshot shows the `CurrentItem` and `CurrentState` properties of the `Shell.Current` object:

![Screenshot of debugger](navigation-images/debugger.png "Debugger")

In this example, the `CurrentItem` property, of type `FlyoutItem`, displays the title and route of the `FlyoutItem` object. Similarly, the `CurrentState` property, of type `ShellNavigationState`, displays the URI of the displayed route within the Shell application.

### Tab class

The `Tab` class defines a `Stack` property, of type `IReadOnlyList<Page>`, which represents the current pushed navigation stack within the `Tab`. The class also provides the following overridable navigation methods:

- `GetNavigationStack`, returns `IReadOnlyList<Page`>, the current navigation stack.
- `OnInsertPageBefore`, that's called when `INavigation.InsertPageBefore` is called.
- `OnPopAsync`, returns `Task<Page>`, and is called when `INavigation.PopAsync` is called.
- `OnPopToRootAsync`, returns `Task`, and is called when `INavigation.OnPopToRootAsync` is called.
- `OnPushAsync`, returns `Task`, and is called when `INavigation.PushAsync` is called.
- `OnRemovePage`, that's called when `INavigation.RemovePage` is called.

For example, the following code example shows how to override the `OnRemovePage` method:

```csharp
public class MyTab : Tab
{
    protected override void OnRemovePage(Page page)
    {
        base.OnRemovePage(page);

        // Custom logic
    }
}
```

`MyTab` objects can then be consumed in your Shell visual hierarchy instead of `Tab` objects.

## Navigation events

The `Shell` class defines a `Navigating` event, which is fired when navigation is about to be performed, either due to programmatic navigation or user interaction. The `ShellNavigatingEventArgs` object that accompanies the `Navigating` event provides the following properties:

| Property | Type | Description |
|---|---|---|
| `Current` | `ShellNavigationState` | The URI of the current page. |
| `Source` | `ShellNavigationSource` | The type of navigation that occurred. |
| `Target` | `ShellNavigationState`  | The URI representing where the navigation is destined. |
| `CanCancel`  | `bool` | A value indicating if it's possible to cancel the navigation. |
| `Cancelled`  | `bool` | A value indicating if the navigation was cancelled. |

In addition, the `ShellNavigatingEventArgs` class provides a `Cancel` method that can be used to cancel navigation, and a `GetDeferral` method that returns a `ShellNavigatingDeferral` token that can be used to complete navigation. For more information about navigation deferral, see [Navigation deferral](#navigation-deferral).

The `Shell` class also defines a `Navigated` event, which is fired when navigation has completed. The `ShellNavigatedEventArgs` object that accompanies the `Navigating` event provides the following properties:

| Property | Type | Description |
|---|---|---|
| `Current` | `ShellNavigationState` | The URI of the current page. |
| `Previous`| `ShellNavigationState` | The URI of the previous page. |
| `Source`  | `ShellNavigationSource` | The type of navigation that occurred. |

The `ShellNavigatedEventArgs` and `ShellNavigatingEventArgs` classes both have `Source` properties, of type `ShellNavigationSource`. This enumeration provides the following values:

- `Unknown`
- `Push`
- `Pop`
- `PopToRoot`
- `Insert`
- `Remove`
- `ShellItemChanged`
- `ShellSectionChanged`
- `ShellContentChanged`

Therefore, in a handler for the `Navigating` event, it's possible to intercept navigation and performs actions based on the navigation source. For example, the following code shows how to cancel backwards navigation if the data on the page is unsaved:

```csharp
void OnNavigating(object sender, ShellNavigatingEventArgs e)
{
    // Cancel back navigation if data is unsaved
    if (e.Source == ShellNavigationSource.Pop && !dataSaved)
    {
        e.Cancel();
    }
}
```

## Navigation deferral

Shell navigation can be intercepted and completed or cancelled based on user choice. This can be achieved by overriding the `OnNavigating` method in your `Shell` subclass, and by calling the `GetDeferral` method on the `ShellNavigatingEventArgs` object. This method returns a `ShellNavigatingDeferral` token that has a `Complete` method, which can be used to complete the navigation request:

```csharp
public MyShell : Shell
{
    // ...
    protected override async void OnNavigating(ShellNavigatingEventArgs args)
    {
        base.OnNavigating(args);

        ShellNavigatingDeferral token = args.GetDeferral();
        var result = await DisplayActionSheet("Navigate?", "Cancel", "Yes", "No");

        if (result != "Yes")
        {
            args.Cancel();
        }
        token.Complete();
    }    
}
```

In this example, an action sheet is displayed that invites the user to complete the navigation request, or cancel it. Navigation is cancelled by invoking the `Cancel` method on the `ShellNavigatingEventArgs` object. Navigation is completed by invoking the `Complete` method on the `ShellNavigatingDeferral` token that was retrieved by the `GetDeferral` method on the `ShellNavigatingEventArgs` object.

> [!IMPORTANT]
> The `GoToAsync` method will throw a `InvalidOperationException` if a user tries to navigate while there is a pending navigation deferral.

## Pass data

Data can be passed as query parameters when performing programmatic navigation. For example, the following code is executed in the sample application when a user selects an elephant on the `ElephantsPage`:

```csharp
async void OnCollectionViewSelectionChanged(object sender, SelectionChangedEventArgs e)
{
    string elephantName = (e.CurrentSelection.FirstOrDefault() as Animal).Name;
    await Shell.Current.GoToAsync($"//animals/elephants/elephantdetails?name={elephantName}");
}
```

This code example retrieves the currently selected elephant in the [`CollectionView`](xref:Xamarin.Forms.CollectionView), and navigates to the `elephantdetails` route, passing `elephantName` as a query parameter. Note that query parameters will be URL encoded for navigation, so "Indian Elephant" will become "Indian%20Elephant".

To receive data, the class that represents the page being navigated to, or the class for the page's [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext), must be decorated with a `QueryPropertyAttribute` for each query parameter:

```csharp
[QueryProperty("Name", "name")]
public partial class ElephantDetailPage : ContentPage
{
    public string Name
    {
        set
        {
            BindingContext = ElephantData.Elephants.FirstOrDefault(m => m.Name == Uri.UnescapeDataString(value));
        }
    }
    ...
}
```

The first argument for the `QueryPropertyAttribute` specifies the name of the property that will receive the data, with the second argument specifying the query parameter id. Therefore, the `QueryPropertyAttribute` in the above example specifies that the `Name` property will receive the data passed in the `name` query parameter from the URI in the `GoToAsync` method call. The `Name` property then URL decodes the query parameter value, and uses it to set the [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) of the page to the object that will be displayed.

> [!NOTE]
> A class can be decorated with multiple `QueryPropertyAttribute` objects.

## Back button behavior

The `BackButtonBehavior` class defines the following properties that control back button appearance and behavior:

- `Command`, of type `ICommand`, which is executed when the back button is pressed.
- `CommandParameter`, of type `object`, which is the parameter that's passed to the `Command`.
- `IconOverride`, of type [`ImageSource`](xref:Xamarin.Forms.ImageSource), the icon used for the back button.
- `IsEnabled`, of type `boolean`, indicates whether the back button is enabled. The default value is `true`.
- `TextOverride`, of type `string`, the text used for the back button.

All of these properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects, which means that the properties can be targets of data bindings.

The `BackButtonBehavior` class can be consumed by setting the `Shell.BackButtonBehavior` attached property to a `BackButtonBehavior` object:

```xaml
<ContentPage ...>    
    <Shell.BackButtonBehavior>
        <BackButtonBehavior Command="{Binding BackCommand}"
                            IconOverride="back.png" />   
    </Shell.BackButtonBehavior>
    ...
</ContentPage>
```

The equivalent C# code is:

```csharp
Shell.SetBackButtonBehavior(this, new BackButtonBehavior
{
    Command = new Command(() =>
    {
        ...
    }),
    IconOverride = "back.png"
});
```

The `Command` property is set to an `ICommand` to be executed when the back button is pressed, and the `IconOverride` property is set to the icon that's used for the back button:

[![Screenshot of a Shell back button icon override, on iOS and Android](navigation-images/back-button.png "Shell back button icon override")](navigation-images/back-button-large.png#lightbox "Shell back button icon override")

## Related links

- [Xaminals (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
