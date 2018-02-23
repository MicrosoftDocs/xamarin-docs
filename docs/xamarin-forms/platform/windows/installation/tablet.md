---
title: "Adding a Windows App"
ms.topic: article
ms.prod: xamarin
ms.assetid: ADF99B78-F1BC-48DF-9128-01B93C4411C1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
---

# Adding a Windows App


1. **right-click on solution > Add > New Project...** and add a **Blank App (Windows)**

 ![](tablet-images/add-wu.png "Add New Project Dialog")

2. **right-click on project > Manage NuGet Packages...** and
  add the **Xamarin.Forms** package.

3. **right-click on project > Add > Reference** and create a project
  reference to the shared Xamarin.Forms application project.

  ![](tablet-images/addref.png "Reference Manager Dialog")

4. Edit **App.xaml.cs** to include the `Init()` method call
    inside the `OnLaunched` method around line 65

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5 . Edit **MainPage.xaml** - change the root element `<Page` to `<forms:WindowsPage` *and*
  define the `xmlns:forms` that it uses:

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPage>
```


 6 . Edit **MainPage.xaml.cs** to remove the `: Page` inheritance specifier
  for the class name.

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 7 . Still in **MainPage.xaml.cs**, add the `LoadApplication` call
  in the `MainPage` constructor (around line 28) to start your Xamarin.Forms app:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location

9 . Finally, add any local resources (eg. image files) from
  the existing platform projects that are required.

