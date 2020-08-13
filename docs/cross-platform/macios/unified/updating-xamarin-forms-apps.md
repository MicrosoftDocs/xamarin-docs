---
title: "Updating Existing Xamarin.Forms Apps"
description: "This document describes the steps that must be followed to update a Xamarin.Forms app from the Classic API to the Unified API."
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
---

# Updating Existing Xamarin.Forms Apps

_Follow these steps to update an existing Xamarin.Forms app to use the Unified API and update to version 1.3.1_

> [!IMPORTANT]
> Because Xamarin.Forms 1.3.1 is the first release that supports the Unified API, the entire solution should be updated to use the latest version at the same time as migrating the iOS app to Unified. This means that in addition to updating the iOS project for Unified support, you'll also need to edit code in _all_ the projects in the solution.

The update is performed in two steps:

1. Migrate the iOS app to the Unified API using Visual Studio for Mac's build in migration tool.

    - Use the migration tool to automatically update the project.

    - Update iOS native APIs as outlined in the instructions to [update iOS apps](~/cross-platform/macios/unified/updating-ios-apps.md) (specifically in custom renderer or dependency service code).

2. Update the entire solution to Xamarin.Forms version 1.3.

    1. Install the Xamarin.Forms 1.3.1 NuGet package.

    2. Update the `App` class in the shared code.

    3. Update the `AppDelegate` in the iOS project.

    4. Update the `MainActivity` in the Android project.

    5. Update the `MainPage` in the Windows Phone project.

## 1. iOS App (Unified Migration)

Part of the migration requires upgrading Xamarin.Forms to version 1.3, which supports the Unified API. In order for the correct assembly references to be created, we first need to update the iOS project to use the Unified API.

### Migration Tool

Click on the iOS project so that it's selected, then choose **Project > Migrate to Xamarin.iOS Unified API...** and agree to the warning message that appears.

![Choose Project > Migrate to Xamarin.iOS Unified API... and agree to the warning message that appears](updating-xamarin-forms-apps-images/beta-tool1.png)

This will automatically:

- Change the project type to support the Unified 64-bit API.
- Change the framework reference to **Xamarin.iOS** (replacing the old **monotouch** reference).
- Change the namespace references in the code to remove the `MonoTouch` prefix.
- Update the **csproj** file to use the correct build targets for the Unified API.

**Clean** and **Build** the project to ensure there are no other errors to fix. No further action should be required. These steps are explained in more detail in the [Unified API docs](~/cross-platform/macios/unified/updating-ios-apps.md).

### Update native iOS APIs (if required)

If you have added additional iOS native code (such as custom renderers or dependency services) you may need to perform additional manual code fixes. Re-compile your app and refer to the [Updating Existing iOS Apps instructions](~/cross-platform/macios/unified/updating-ios-apps.md) for additional information on changes that may be required. [These tips](~/cross-platform/macios/unified/updating-tips.md) will also help identify changes that are required.

## 2. Xamarin.Forms 1.3.1 Update

Once the iOS app has been updated to the Unified API, the rest of the solution needs to be updated to Xamarin.Forms version 1.3.1. This includes:

- Updating the Xamarin.Forms NuGet package in each project.
- Changing the code to use the new Xamarin.Forms `Application`,  `FormsApplicationDelegate` (iOS), `FormsApplicationActivity` (Android), and `FormsApplicationPage` (Windows Phone) classes.

These steps are explained below:

### 2.1 Update NuGet in all Projects

Update Xamarin.Forms to 1.3.1 pre-release using the NuGet Package Manager for all projects in the solution: PCL (if present), iOS, Android, and Windows Phone. It is recommended that you **delete and re-add** the Xamarin.Forms NuGet package to update to version 1.3.

> [!NOTE]
> Xamarin.Forms version 1.3.1 is currently in *pre-release*. This means you must select the **pre-release** option in NuGet (via a tick-box in Visual Studio for Mac or a drop-down-list in Visual Studio) to see the latest pre-release version.

> [!IMPORTANT]
> If you are using Visual Studio, ensure the latest version of the NuGet Package Manager is installed. Older versions of NuGet in Visual Studio will not correctly install the Unified version of Xamarin.Forms 1.3.1. Go to **Tools > Extensions and Updates...** and click on the **Installed** list to check that the **NuGet Package Manager for Visual Studio** is at least version 2.8.5. If it is older, click on the **Updates** list to download the latest version.

Once you've updated the NuGet package to Xamarin.Forms 1.3.1, make the following changes in each project to upgrade to the new `Xamarin.Forms.Application` class.

### 2.2 Portable Class Library (or Shared Project)

Change the **App.cs** file so that:

- The `App` class now inherits from `Application`.
- The `MainPage` property is set to the first content page you wish to display.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

We have completely removed the `GetMainPage` method, and instead set the `MainPage` *property* on the `Application` subclass.

This new `Application` base class also supports the `OnStart`, `OnSleep`, and `OnResume` overrides to help you manage your application's lifecycle.

The `App` class is then passed to a new `LoadApplication` method in each app project, as described below:

### 2.3 iOS App

Change the **AppDelegate.cs** file so that:

- The class inherits from `FormsApplicationDelegate` (instead of `UIApplicationDelegate` previously).
- `LoadApplication` is called with a new instance of `App`.

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

### 2.3 Android App

Change the **MainActivity.cs** file so that:

- The class inherits from `FormsApplicationActivity` (instead of `FormsActivity` previously).
- `LoadApplication` is called with a new instance of `App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
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

### 2.4 Windows Phone App

We need to update the **MainPage** - both the XAML and the codebehind.

Change the **MainPage.xaml** file so that:

- The root XAML element should be `winPhone:FormsApplicationPage`.
- The `xmlns:phone` attribute should be *changed* to `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

An updated example is shown below - you should only have to edit these things (the rest of the attributes should remain the same):

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

Change the **MainPage.xaml.cs** file so that:

- The class inherits from `FormsApplicationPage` (instead of `PhoneApplicationPage` previously).
- `LoadApplication` is called with a new instance of the Xamarin.Forms `App` class. You may need to fully qualify this reference since Windows Phone has it's own `App` class already defined.

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

### Troubleshooting

Occasionally you will see an error similar to this after updating the Xamarin.Forms NuGet package. It occurs when the NuGet updater does not completely remove references to older versions from your **csproj** files.

>YOUR\_PROJECT.csproj: Error: This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see https://go.microsoft.com/fwlink/?LinkID=322105. The missing file is ../../packages/Xamarin.Forms.1.2.3.6257/build/portable-win+net45+wp80+MonoAndroid10+MonoTouch10/Xamarin.Forms.targets. (YOUR\_PROJECT)

To fix these errors, open the **csproj** file in a text editor and look for `<Target` elements that refer to older versions of Xamarin.Forms, such as the element shown below. You should manually delete this entire element from the **csproj** file and save the changes.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see https://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

The project should build successfully once these old references are removed.

## Considerations

The following considerations should be taken into account when converting an existing Xamarin.Forms project from the Classic API to the new Unified API if that app relies on one or more Component or NuGet Package.

### Components

Any component that you have included in your application will also need to be updated to the Unified API or you will get a conflict when you try to compile. For any included component, replace the current version with a new version from the Xamarin Component Store that supports the Unified API and do a clean build. Any component that has not yet been converted by the author, will display a 32 bit only warning in the component store.

### NuGet Support

While we contributed changes to NuGet to work with the Unified API support, there has not been a new release of NuGet, so we are evaluating how to get NuGet to recognize the new APIs.

Until that time, just like the components, you'll need to switch any NuGet Package you have included in your project to a version that supports the Unified APIs and do a clean build afterwards.

> [!IMPORTANT]
> If you have an error in the form _"Error 3 Cannot include both 'monotouch.dll' and 'Xamarin.iOS.dll' in the same Xamarin.iOS project - 'Xamarin.iOS.dll' is referenced explicitly, while 'monotouch.dll' is referenced by 'xxx, Version=0.0.000, Culture=neutral, PublicKeyToken=null'"_ after converting your application to the Unified APIs, it is typically due to having either a component or NuGet Package in the project that has not been updated to the Unified API. You'll need to remove the existing component/NuGet, update to a version that supports the Unified APIs and do a clean build.

## Enabling 64 Bit Builds of Xamarin.iOS Apps

For a Xamarin.iOS mobile application that has been converted to the Unified API, the developer still needs to enable the building of the application for 64 bit machines from the app's Options. Please see the **Enabling 64 Bit Builds of Xamarin.iOS Apps** of the [32/64 bit Platform Considerations](~/cross-platform/macios/32-and-64/index.md#enable-64) document for detailed instructions on enabling 64 bit builds.

## Summary

The Xamarin.Forms application should now be updated to version 1.3.1 and the iOS app migrated to the Unified API (which supports 64-bit architectures on the iOS platform).

As noted above, if your Xamarin.Forms app includes native code such as custom renderers or dependency services then these may also need updating to use the new Types [introduced in the Unified API](~/cross-platform/macios/index.md).

## Related Links

- [Updating iOS Apps](~/cross-platform/macios/unified/updating-apps.md)
- [Updating iOS Apps](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Working with Native Types in Cross-Platform Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Updating Tips](~/cross-platform/macios/unified/updating-tips.md)
- [Classic vs Unified API differences](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
