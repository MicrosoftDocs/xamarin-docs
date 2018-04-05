---
title: "App Class"
description: "Features of the default App class, which can be either C# or XAML"
ms.prod: xamarin
ms.assetid: 421F8294-1944-46A4-8459-D2BD5AAABC9D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
---

# App Class

The `Application` base class offers the following features, which are exposed
in your projects default `App` subclass:

* A `MainPage` property, which is where to set the initial page for the app.
* A persistent [`Properties` dictionary](#Properties_Dictionary) to store simple values across lifecycle state changes.
* A static `Current` property that contains a reference to the current application object.

It also exposes [Lifecycle methods](~/xamarin-forms/app-fundamentals/app-lifecycle.md) such
as `OnStart`, `OnSleep`, and `OnResume` as well as modal navigation events.

Depending on which template you chose, the `App` class could be defined in one
of two ways:

* **C#**, or
* **XAML & C#**

To create an **App** class using XAML, the default **App** class must be replaced with a XAML **App** class and associated code-behind, as shown in the following code example:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Photos.App">

</Application>
```

The following code example shows the associated code-behind:

```csharp
public partial class App : Application
{
	public App ()
	{
		InitializeComponent ();
		MainPage = new HomePage ();
	}
	...
}
```

As well as setting the [`MainPage`](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) property, the code-behind must also call the `InitializeComponent` method to load and parse the associated XAML.

## MainPage Property

The `MainPage` property on the `Application` class sets the root page of the application.

For example, you can create logic in your `App` class to display a different page depending on whether the user is logged in or not.

The `MainPage` property should be set in the `App` constructor,

```csharp
public class App : Xamarin.Forms.Application
{
	public App ()
	{
		MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
	}
}
```

<a name="Properties_Dictionary" />

## Properties Dictionary

The `Application` subclass has a static `Properties` dictionary which can be used to store data, in particular for use in the `OnStart`, `OnSleep`, and `OnResume` methods. This can be accessed from anywhere in your Xamarin.Forms code using `Application.Current.Properties`.

The `Properties` dictionary uses a `string` key and stores an `object` value.

For example, you could set a persistant `"id"` property anywhere in your code
(when an item is selected, in a page's `OnDisappearing` method, or in the `OnSleep` method)
like this:

```csharp
Application.Current.Properties ["id"] = someClass.ID;
```

In the `OnStart` or `OnResume` methods you can then use this value to recreate
the user's experience in some way. The `Properties` dictionary stores `object`s
so you need to cast its value before using it.

```csharp
if (Application.Current.Properties.ContainsKey("id"))
{
	var id = Application.Current.Properties ["id"] as int;
	// do something with id
}
```

Always check for the presence of the key before accessing it to prevent unexpected errors.

> [!NOTE]
> The `Properties` dictionary can only serialize
> primitive types for storage. Attempting to store other types (such as
> `List<string>`) can fail silently.

<!-- bugzilla 28657 -->

### Persistence

The `Properties` dictionary is saved to the device automatically.
  Data added to the dictionary will be available when the application returns from
	the background or even after it is restarted.

Xamarin.Forms 1.4 introduced an additional method on the `Application` class -
  `SavePropertiesAsync()` - which can be called to
  proactively persist the `Properties` dictionary. This is to allow
  you to save properties after important updates
  rather than risk them not getting serialized out
  due to a crash or being killed by the OS.

You can find references to using the `Properties` dictionary in the
**Creating Mobile Apps with Xamarin.Forms** book chapters [6](https://developer.xamarin.com/r/xamarin-forms/book/chapter06.pdf),
[15](https://developer.xamarin.com/r/xamarin-forms/book/chapter15.pdf), and [20](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf),
and in the associated
[samples](https://github.com/xamarin/xamarin-forms-book-preview-2).



## The Application Class

A complete `Application` class implementation is shown below for reference:

```csharp
public class App : Xamarin.Forms.Application
{
	public App ()
	{
		MainPage = new ContentPage { Title = "App Lifecycle Sample" }; // your page here
	}

	protected override void OnStart()
	{
		// Handle when your app starts
		Debug.WriteLine ("OnStart");
	}

	protected override void OnSleep()
	{
		// Handle when your app sleeps
		Debug.WriteLine ("OnSleep");
	}

	protected override void OnResume()
	{
		// Handle when your app resumes
		Debug.WriteLine ("OnResume");
	}
}

```

This class is then instantiated in each platform-specific project and passed to the
`LoadApplication` method which is where the `MainPage` is loaded and displayed to the user.
The code for each platform is shown in the following sections. The latest Xamarin.Forms solution templates already contain all this code, preconfigured for your app.


### iOS Project

The iOS `AppDelegate` class now inherits from `FormsApplicationDelegate`. It should:

* Call `LoadApplication` with an instance of the `App` class.

* Always return `base.FinishedLaunching (app, options);`.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### Android Project

The Android `MainActivity` now inherits from `FormsApplicationActivity`. In the `OnCreate` override the `LoadApplication` method is called with an instance of the `App` class.

```csharp
[Activity (Label = "App Lifecycle Sample", Icon = "@drawable/icon", MainLauncher = true,
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

> [!NOTE]
> There is a newer [`FormsAppCompatActivity`](~/xamarin-forms/platform/android/appcompat.md)
> base class that can be used to better support Android Material Design.
> This will become the default Android template in future, but you can follow
> [these instructions](~/xamarin-forms/platform/android/appcompat.md)
> to update your existing Android apps.


### Windows Phone Project

The main page in the Windows Phone (Silverlight-based) project should inherit
from `FormsApplicationPage`. This means the XAML and C# for `MainPage` reference
the `FormsApplicationPage` class as shown.

The XAML uses a custom namespace so that the root element reflects the `FormsApplicationPage` class:

```xaml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

The C# inherits from the `FormsApplicationPage` class, and calls `LoadApplication` to create an instance of your Xamarin.Forms `App`. Note that it is good practice to explicitly use the application namespace to qualify the `App` because Windows Phone applications also have their own `App` class unrelated to Xamarin.Forms.

```csharp
public partial class MainPage :
    global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3, use the correct namespace
    }
 }
```

### Windows 8.1 Project

The main page in [Windows 8.1 (WinRT-based)](~/xamarin-forms/platform/windows/installation/tablet.md)
projects should now inherit from  `WindowsPage`. This means the XAML for `MainPage`
reference the `WindowsPage` class as shown:

The XAML uses a custom namespace so that the root element reflects the `FormsApplicationPage` class:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
   ...>
</forms:WindowsPage>
```

The C# codebehind's construction must call `LoadApplication` to create an
instance of your Xamarin.Forms `App`. Note that it is good practice to
explicitly use the application namespace to qualify the `App` because UWP applications
also have their own `App` class unrelated to Xamarin.Forms.

```csharp
public partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();
        LoadApplication(new YOUR_APP_NAMESPACE.App());
    }
 }
```

Note that `Forms.Init()` must be called in **App.xaml.cs** around line 65.

### Universal Windows Project (UWP) for Windows 10

[Universal Windows Project](~/xamarin-forms/platform/windows/installation/universal.md)
support in Xamarin.Forms is currently in Preview.

The main page in the UWP project should inherit from `WindowsPage`. This means
the XAML and C# for `MainPage` reference the `FormsApplicationPage` class as shown.

The XAML uses a custom namespace so that the root element reflects the `FormsApplicationPage` class:

```xaml
<forms:WindowsPage
   ...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
   ...>
</forms:WindowsPage>
```

The C# codebehind's construction must call `LoadApplication` to create an
instance of your Xamarin.Forms `App`. Note that it is good practice to
explicitly use the application namespace to qualify the `App` because UWP applications
also have their own `App` class unrelated to Xamarin.Forms.

```csharp
public sealed partial class MainPage
{
    public MainPage()
    {
        InitializeComponent();

        LoadApplication(new YOUR_NAMESPACE.App());
    }
 }
```

Note that `Forms.Init()` must be called in **App.xaml.cs** around line 63.
