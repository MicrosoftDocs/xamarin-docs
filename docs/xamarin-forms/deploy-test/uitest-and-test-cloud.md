---
title: "Automating Xamarin.Forms Testing with App Center"
description: "Xamarin UITest component can be used with Xamarin.Forms to write UI tests to run in the cloud on hundreds of devices."
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: b674db3d-c526-4e31-a9f4-b6d6528ce7a9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
---

# Automate Xamarin.Forms testing with App Center

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/)

_Xamarin UITest component can be used with Xamarin.Forms to write UI tests to run in the cloud on hundreds of devices._

## Overview

**[App Center Test](/appcenter/test-cloud/)** allows developers to write automated user interface tests for iOS and Android apps. With some minor tweaks, Xamarin.Forms apps can be tested using Xamarin.UITest, including sharing the same test code. This article introduces specific tips to get Xamarin.UITest working with Xamarin.Forms.

This guide does assume that familiarity with Xamarin.UITest. The following guides are recommended for gaining familiarity with Xamarin.UITest:

- [Introduction to App Center Test](/appcenter/test-cloud/)
- [Introduction to UITest](/appcenter/test-cloud/preparing-for-upload/uitest/)

Once a UITest project has been added to a Xamarin.Forms solution, the steps for writing and running the tests for a Xamarin.Forms application are the same as for a Xamarin.Android or Xamarin.iOS application.

## Requirements

Refer to the [Xamarin.UITest](/appcenter/test-cloud/uitest/) to confirm your project is ready for automated UI testing.

## Adding UITest Support to Xamarin.Forms Apps

UITest automates the user interface by activating controls on the screen and performing input anywhere a user would normally interact with the application. To enable tests that can *press a button* or *enter text in a box* the test code will need a way to identify the controls on the screen.

To enable the UITest code to reference controls, each control needs a unique identifier. In Xamarin.Forms, the recommended way to set this identifier is by using the [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) property as shown below:

```csharp
var b = new Button {
    Text = "Click me",
    AutomationId = "MyButton"
};
var l = new Label {
    Text = "Hello, Xamarin.Forms!",
    AutomationId = "MyLabel"
};
```

The [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) property can also be set in XAML:

```xaml
<Button x:Name="b" AutomationId="MyButton" Text="Click me"/>
<Label x:Name="l" AutomationId="MyLabel" Text="Hello, Xamarin.Forms!" />
```

> [!NOTE]
> [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) is a [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) and so can also be set with a binding expression.

A unique [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) should be added to all controls that are required for testing (including buttons, text entries, and labels whose value might need to be queried).

> [!WARN]
> Note that an `InvalidOperationException` will be thrown if an attempt is made to set the [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) property of an [`Element`](xref:Xamarin.Forms.Element) more than once.

### iOS Application Project

To run tests on iOS ,the [Xamarin Test Cloud Agent NuGet package](https://www.nuget.org/packages/Xamarin.TestCloud.Agent/) must be added to the project. Once it has been added, copy the following code into the `AppDelegate.FinishedLaunching` method:

```csharp
#if ENABLE_TEST_CLOUD
// requires Xamarin Test Cloud Agent
Xamarin.Calabash.Start();
#endif
```

The Calabash assembly makes uses of non-public Apple API's which will cause apps to be rejected by the App Store. However, the Xamarin.iOS linker will remove the Calabash assembly from the final IPA if it isn't explicitly referenced from code.

> [!NOTE]
> Release builds do not have the `ENABLE_TEST_CLOUD` compiler variable, which will cause the Calabash assembly to be removed from app bundle. However, debug builds do have the compiler directive defined, preventing the linker from removing the assembly.

The following screenshot shows the `ENABLE_TEST_CLOUD` compiler variable set for Debug builds:

::: zone pivot="windows"

![](uitest-and-test-cloud-images/12-compiler-directive-vs.png "Build Options")

::: zone-end
::: zone pivot="macos"

![](uitest-and-test-cloud-images/11-compiler-directive-xs.png "Compiler Options")

::: zone-end

### Android Application Project

Unlike iOS, Android projects do not need any special startup code.

## Writing UITests

For information about writing UITests, see [UITest documentation](/appcenter/test-cloud/preparing-for-upload/uitest/). The steps below are a summary, specifically describing how the [Xamarin.Forms demo **UsingUITest**](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/) is built.

### Use AutomationId in the Xamarin.Forms UI

Before any UITests can be written, the Xamarin.Forms application user interface must be scriptable. Ensure that all controls in the user interface have a [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) so that they can be referenced in test code.

#### Referring to the AutomationId in UITests

When writing UITests, the [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) value is exposed differently on each platform:

- **iOS** uses the `id` field.
- **Android** uses the `label` field.

To write cross-platform UITests that will find the [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) on both iOS and Android, use the `Marked` test query:

```csharp
app.Query(c=>c.Marked("MyButton"))
```

The shorter form `app.Query("MyButton")` also works.

### Adding a UITest Project to an Existing Solution

::: zone pivot="windows"

Visual Studio has a template to help add a Xamarin.UITest project to an existing Xamarin.Forms solution:

1. Right click on the solution, and select **File > New Project**.
1. From the **Visual C#** Templates, select the **Test** category. Select the **UI Test App > Cross-Platform** template:

    ![Add New Project](uitest-and-test-cloud-images/08-new-uitest-project-vs.png "Add New Project")

    This will add a new project with the **NUnit**, **Xamarin.UITest**, and **NUnitTestAdapter** NuGet packages to the solution:

    ![NuGet Package Manager](uitest-and-test-cloud-images/09-new-uitest-project-xs.png "NuGet Package Manager")

    The **NUnitTestAdapter** is a third party test runner that allows Visual Studio to run NUnit tests from Visual Studio.

    The new project also has two classes in it. **AppInitializer** contains code to help initialize and setup tests. The other class, **Tests**, contains boilerplate code to help start the UITests.

1. Add a project reference from the UITest project to the Xamarin.Android project:

    ![Project Reference Manager](uitest-and-test-cloud-images/10-test-apps-vs.png "Project Reference Manager")

    This will allow the **NUnitTestAdapter** to run the UITests for the Android app from Visual Studio.

::: zone-end
::: zone pivot="macos"

It is possible to add a new Xamarin.UITest project to an existing solution manually:

1. Start by adding a new project by selecting the solution, and clicking **File > Add New Project**. In the **New Project** dialog, select **Cross-platform > Tests > Xamarin Test Cloud > UI Test App**:

    ![Choose a Template](uitest-and-test-cloud-images/02-new-uitest-project-xs.png "Choose a Template")

    This will add a new project that already has the **NUnit** and **Xamarin.UITest** NuGet packages in the solution:

    ![Xamarin UITest NuGet Packages](uitest-and-test-cloud-images/03-new-uitest-project-xs.png "Xamarin UITest NuGet Packages")

    The new project also has two classes in it. **AppInitializer** contains code to help initialize and setup tests. The other class, **Tests**, contains boilerplate code to help start the UITests.

1. Select **View > Pads > Unit Tests** to display the Unit Test pad. Expand **UsingUITest > UsingUITest.UITests > Test Apps**:

    ![Unit Test Pad](uitest-and-test-cloud-images/04-unit-test-pad-xs.png "Unit Test Pad")

1. Right click on **Test Apps**, click on **Add App Project**, and select iOS and Android projects in the dialog that appears:

    ![Test Apps Dialog](uitest-and-test-cloud-images/05-add-test-apps-xs.png "Test Apps Dialog")

    The **Unit Test** pad should now have a reference to the iOS and Android projects. This will allow the Visual Studio for Mac test runner to execute UITests locally against the two Xamarin.Forms projects.

#### Adding UITest to the iOS App

There are some additional changes that need to be performed to the iOS application before Xamarin.UITest will work:

1. Add the **Xamarin Test Cloud Agent** NuGet package. Right click on **Packages**, select **Add Packages**, search NuGet for the **Xamarin Test Cloud Agent** and add it to the Xamarin.iOS project:

    ![Add NuGet Packages](uitest-and-test-cloud-images/07-add-test-cloud-agent-xs.png "Add NuGet Packages")

1. Edit the `FinishedLaunching` method of the **AppDelegate** class to initialize the Xamarin Test Cloud Agent when the iOS application starts, and to set the `AutomationId` property of the views. The `FinishedLaunching` method should resemble the following code example:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    #if ENABLE_TEST_CLOUD
    Xamarin.Calabash.Start();
    #endif

    global::Xamarin.Forms.Forms.Init();

    LoadApplication(new App());

    return base.FinishedLaunching(app, options);
}
```

::: zone-end

After adding Xamarin.UITest to the Xamarin.Forms solution, it's possible to create UITests, run them locally, and submit them to Xamarin Test Cloud.

## Summary

Xamarin.Forms applications can be easily tested with **Xamarin.UITest** using a simple mechanism to expose the [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) as a unique view identifier for test automation. Once a UITest project has been added to a Xamarin.Forms solution, the steps for writing and and running the tests for a Xamarin.Forms application are the same as for a Xamarin.Android or Xamarin.iOS application.

For information about how to submit tests to App Center Test, see [Submitting UITests](/appcenter/test-cloud/preparing-for-upload/uitest/). For more information about UITest, see [App Center Test documentation](/appcenter/test-cloud/).

## Related Links

- [UITestSample](https://developer.xamarin.com/samples/xamarin-forms/UsingUITest/)
- [Xamarin.Forms Samples](https://github.com/xamarin/xamarin-forms-samples)
- [App Center Test](/appcenter/test-cloud/)
- [Xamarin.UITest](/appcenter/test-cloud/uitest/)
- [NUnit](http://www.nunit.org)
