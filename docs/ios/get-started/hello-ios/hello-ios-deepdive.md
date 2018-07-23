---
title: "Hello, iOS – Deep dive"
description: "This document takes a deeper look at the Hello, iOS sample application, considering its architecture, user interface, content view hierarchy, testing, deployment, and more."
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 61ba3a7e-fe11-4439-8bc8-9809512b8eff
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
---
# Hello, iOS – Deep dive

The Quickstart walkthrough, introduced building and running a basic Xamarin.iOS application. Now it’s time to develop a deeper understanding of how iOS applications work so you can build more sophisticated programs. This guide reviews the steps that in the Hello, iOS walkthrough to enable understanding of the fundamental concepts of iOS application development.

This guide helps you develop the skills and knowledge required to build a single-screen iOS application. After working through it, you should have an understanding of the different parts of a Xamarin.iOS application and how they fit together.

# [Visual Studio for Mac](#tab/vsmac)

## Introduction to Visual Studio for Mac

Visual Studio for Mac is a free, open-source IDE that combines features from Visual Studio and XCode. It features a fully integrated visual designer, a text editor complete with refactoring tools, an assembly browser, source code integration, and more. This guide introduces some basic Visual Studio for Mac features, but if you're new to Visual Studio for Mac, check out the [Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/) documentation.

Visual Studio for Mac follows the Visual Studio practice of organizing code into *Solutions* and *Projects*. A Solution is a container that can hold one or more Projects. A Project can be an application (such as iOS or Android), a supporting library, a test application, and more. In the Phoneword app, a new iPhone Project was added using the **Single View Application** template. The initial Solution looked like this:

![](hello-ios-deepdive-images/image30.png "A screenshot of the initial Solution")

# [Visual Studio](#tab/vswin)

## Introduction to Visual Studio

Visual Studio is a powerful IDE from Microsoft. It features a fully integrated visual designer, a text editor complete with refactoring tools, an assembly browser, source code integration, and more. This guide introduces some basic Visual Studio features with the Xamarin plug-in.

Visual Studio organizes code into _Solutions_ and *Projects*. A Solution is a container that can hold one or more Projects. A Project can be an application (such as iOS or Android), a supporting library, a test application, and more. In the Phoneword app, a new iPhone Project was added using the **Single View Application** template. The initial Solution looked like this:

![](hello-ios-deepdive-images/vs-image30.png "A screenshot of the initial Solution")

-----

## Anatomy of a Xamarin.iOS application

# [Visual Studio for Mac](#tab/vsmac)

On the left is the *Solution Pad*, which contains the directory structure and all the files associated with the Solution:

![](hello-ios-deepdive-images/image31.png "The Solution Pad, which contains the directory structure and all the files associated with the Solution")

# [Visual Studio](#tab/vswin)

On the right is the *Solution Pane*, which contains the directory structure and all the files associated with the Solution:

![](hello-ios-deepdive-images/vs-image31.png "The Solution Pane, which contains the directory structure and all the files associated with the Solution")

-----

In the [Hello, iOS](~/ios/get-started/hello-ios/hello-ios-quickstart.md) walkthrough, you created a Solution called **Phoneword** and placed an iOS Project - **Phoneword_iOS** - inside it. The items inside the Project include:

-  **References** - Contains the assemblies required to build and run the application. Expand the directory to see references to .NET assemblies such as  [System](http://msdn.microsoft.com/library/system%28v=vs.110%29.aspx) , System.Core, and  [System.Xml](http://msdn.microsoft.com/library/system.xml%28v=vs.110%29.aspx) , as well as a reference to Xamarin's Xamarin.iOS assembly.
-  **Packages** - The Packages directory contains ready-made NuGet packages.
-  **Resources** - The Resources folder stores other media.
-  **Main.cs** – This contains the main entry point of the application. To start the application, the name of the main application class, the  `AppDelegate`, is passed in .
-  **AppDelegate.cs** – This file contains the main application class and is responsible for creating the Window, building the user interface, and listening to events from the operating system.
-  **Main.storyboard** - The Storyboard contains the visual design of the application’s user interface. Storyboard files open in a graphical editor called the iOS Designer.
-  **ViewController.cs** – The View Controller powers the screen (View) that a user sees and touches. The View Controller is responsible for handling interactions between the user and the View.
-  **ViewController.designer.cs** – The `designer.cs` is an auto-generated file that serves as the glue between controls in the View and their code representations in the View Controller. Because this is an internal plumbing file, the IDE will overwrite any manual changes and most of the time this file can be ignored. For more information on the relationship between the visual Designer and the backing code, refer to the  [Introduction to the iOS Designer](~/ios/user-interface/designer/introduction.md) guide.
-  **Info.plist** – The `Info.plist` is where application properties such as the application name, icons, launch images, and more are set. This is a powerful file and a thorough introduction to it is available in the  [Working with Property Lists](~/ios/app-fundamentals/property-lists.md) guide.
-  **Entitlements.plist** - The entitlements property list lets us specify application  *capabilities* (also called App Store Technologies) such as iCloud, PassKit, and more. More information on the  `Entitlements.plist` can be found in the  [Working with Property Lists](~/ios/app-fundamentals/property-lists.md) guide. For a general introduction to entitlements, refer to the  [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) guide.

## Architecture and app fundamentals

Before an iOS application can load a user interface, two things need to be in place. First, the application needs to define an *entry point* – the first code that runs when the application’s process is loaded into memory. Second, it needs to define a class to handle application-wide events and interact with the operating system.

This section studies the relationships illustrated in the following diagram:

[![](hello-ios-deepdive-images/image32.png "The Architecture and App Fundamentals relationships are illustrated in this diagram")](hello-ios-deepdive-images/image32.png#lightbox)

Let's start at the beginning and learn what happens at application startup.

### Main

The main entry point of an iOS application is the `Main.cs` file. `Main.cs` contains a static Main method that creates a new Xamarin.iOS application instance and passes the name of the *Application Delegate* class that will handle OS events. The template code for the static `Main` method appears below:

```csharp
using System;
using UIKit;

namespace Phoneword_iOS
{
    public class Application
    {
        static void Main (string[] args)
        {
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### Application delegate

In iOS, the *Application Delegate* class handles system events; this class lives inside `AppDelegate.cs`. The `AppDelegate` class manages the application *Window*. The Window is a single instance of the `UIWindow` class that serves as a container for the user interface. By default, an application gets only one Window onto which to load its content, and the Window is attached to a *Screen* (single `UIScreen` instance) that provides the bounding rectangle matching the dimensions of the physical device screen.

The *AppDelegate* is also responsible for subscribing to system updates about important application events such as when the app finishes launching or when memory is low.

The template code for the AppDelegate is presented below:

```csharp
using System;
using Foundation;
using UIKit;

namespace Phoneword_iOS
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        public override UIWindow Window {
            get;
            set;
        }

        ...
    }
}
```

Once the application has defined its Window, it can begin loading the user interface. The next section explores UI creation.

## User interface

The user interface of an iOS app is like a storefront - the application typically gets one Window, but it can fill the Window up with as many objects at it needs, and the objects and arrangements can be changed depending on what the app wants to display. The objects in this scenario - the things that the user sees - are called Views. To build a single screen in an application, Views are stacked on top of each other in a *Content View Hierarchy*, and the hierarchy is managed by a single View Controller. Applications with multiple screens have multiple Content View Hierarchies, each with its own View Controller, and the application places Views in the Window to create a different Content View Hierarchy based on the screen that the user is on.

This section dives into the user interface by describing Views, Content View Hierarchies, and the iOS Designer.

### iOS Designer and storyboards

The iOS Designer is a visual tool for building user interfaces in Xamarin. The Designer can be launched by double-clicking on any Storyboard (.storyboard) file, which will open to a view that resembles the following screenshot:

# [Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image33.png "iOS Designer Interface")

A *Storyboard* is a file that contains the visual designs of our application’s screens as well as the transitions and relationships between the screens. The representation of an application’s screen in a Storyboard is called a _Scene_. Each Scene represents a View Controller and the stack of Views that it manages (Content View Hierarchy). When a new **Single View Application** Project is created from a template, Visual Studio for Mac automatically generates a Storyboard file called `Main.storyboard` and populates it with a single Scene, as illustrated by the screenshot below:

![](hello-ios-deepdive-images/image34.png "Visual Studio for Mac automatically generates a Storyboard file called Main.storyboard and populates it with a single Scene")

The black bar at the bottom of the Storyboard screen can be selected to choose the View Controller for the Scene. The View Controller is an instance of the `UIViewController` class that contains the backing code for the Content View Hierarchy. Properties on this View Controller can be viewed and set inside the **Properties Pad**, as illustrated by the screenshot below:

![](hello-ios-deepdive-images/image35.png "The Properties Pane")

# [Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image33.png "iOS Designer Interface")

A *Storyboard* is a file that contains the visual designs of our application’s screens as well as the transitions and relationships between the screens. The representation of an application’s screen in a Storyboard is called a _Scene_. Each Scene represents a View Controller and the stack of Views that it manages (Content View Hierarchy). When a new **Single View Application** Project is created from a template, Visual Studio automatically generates a Storyboard file called `Main.storyboard` and populates it with a single Scene, as illustrated by the screenshot below:

![](hello-ios-deepdive-images/vs-image34.png "Visual Studio automatically generates a Storyboard file called Main.storyboard and populates it with a single Scene")

The bar at the bottom of the Storyboard screen can be selected to choose the View Controller for the Scene. The View Controller is an instance of the `UIViewController` class that contains the backing code for the Content View Hierarchy. Properties on this View Controller can be viewed and set inside the **Properties Pane**, as illustrated by the screenshot below:

![](hello-ios-deepdive-images/vs-image35.png "The Properties Pane")

-----

The _View_ can be selected by clicking inside the white part of the Scene. The View is an instance of the `UIView` class that defines an area of the screen and provides interfaces for working with the content in that area. The default View is a single *Root View* that fills the whole device screen.

To the left of the Scene is a gray arrow with a flag icon, as illustrated by the screenshot below:

 [![](hello-ios-deepdive-images/image37.png "A gray arrow with a flag icon")](hello-ios-deepdive-images/image37.png#lightbox)

The gray arrow represents a Storyboard transition called a *Segue* (pronounced “seg-way”). Since this Segue has no origin, it is called a *Sourceless Segue*. A Sourceless Segue points to the first Scene whose Views gets loaded into our application's Window at application startup. The Scene and the Views inside it will be the first thing that the user sees when the app loads.

When building a user interface, additional Views can be dragged from the **Toolbox** onto the main View on the design surface, as illustrated by the screenshot below:

# [Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image38.png "Additional Views can be dragged from the Toolbox onto the main View on the design surface")

# [Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image38.png "Additional Views can be dragged from the Toolbox onto the main View on the design surface")

-----

These additional Views are called *Subviews*. Together, the root View and Subviews are part of a *Content View Hierarchy* that is managed by the `ViewController`. The outline of all the elements in the Scene can be viewed by examining it in the **Document Outline** pad:

# [Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image39.png "The Document Outline pad")

# [Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image39.png "The Document Outline pad")

-----

The Subviews are highlighted in the diagram below:

# [Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image40.png "The Subviews are highlighted in the diagram")

# [Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image40.png "The Subviews are highlighted in the diagram")

-----

The next section breaks down the Content View Hierarchy represented by this Scene.

## Content view hierarchy

A _Content View Hierarchy_ is a stack of Views and Subviews managed by a single View Controller, as illustrated by the diagram below:

 [![](hello-ios-deepdive-images/image41.png "The Content View Hierarchy")](hello-ios-deepdive-images/image41.png#lightbox)

We can make the Content View Hierarchy of our `ViewController` easier to see by temporarily changing the background color of the root View to yellow in the View section of the **Properties Pad**, as illustrated by the screenshot below:

# [Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image42.png "Changing the background color of the root View to yellow in the View section of the Properties Pad")

# [Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image42.png "Changing the background color of the root View to yellow in the View section of the Properties Pad")

-----

The diagram below illustrates the relationships between the Window, Views, Subviews, and View Controller that bring the user interface to the device screen:

 [![](hello-ios-deepdive-images/image43.png "The relationships between the Window, Views, Subviews, and View Controller")](hello-ios-deepdive-images/image43.png#lightbox)

In the next section discusses how to work with Views in code and learn to program for user interaction using View Controllers and the View Lifecycle.

## View controllers and the view lifecycle

Every Content View Hierarchy has a corresponding View Controller to power user interaction. The role of the View Controller is to manage the Views in the Content View Hierarchy. The View Controller is not part of the Content View Hierarchy, and it's not an element in the interface. Rather, it provides the code that powers the user's interactions with the objects on the screen.

### View controllers and storyboards

# [Visual Studio for Mac](#tab/vsmac)

The View Controller is represented in a Storyboard as a bar at the bottom of the Scene. Selecting the View Controller brings up its properties in the **Properties Pad**:

![](hello-ios-deepdive-images/image44.png "Selecting the View Controller brings up its properties in the Properties Pane")

A custom View Controller class for the Content View Hierarchy represented by this Scene can be set by editing the **Class** property in the **Identity** section of the **Properties Pad**. For example, our **Phoneword** application sets the `ViewController` as the View Controller for our first screen, as illustrated by the screenshot below:

![](hello-ios-deepdive-images/image45new.png "The Phoneword application sets the ViewController as the View Controller")

# [Visual Studio](#tab/vswin)

The View Controller is represented in a Storyboard as a bar at the bottom of the Scene. Selecting the View Controller brings up its properties in the **Properties Pane**:

![](hello-ios-deepdive-images/vs-image44.png "Selecting the View Controller brings up its properties in the Properties Pane")

A custom View Controller class for the Content View Hierarchy represented by this Scene can be set by editing the **Class** property in the **Identity** section of the **Properties Pane**. For example, our **Phoneword** application sets the `ViewController` as the View Controller for our first screen, as illustrated by the screenshot below:

![](hello-ios-deepdive-images/vs-image45.png "The Phoneword application sets the ViewController as the View Controller")

-----

This links the Storyboard representation of the View Controller to the `ViewController` C# class. Open the `ViewController.cs` file and notice View Controller is a *subclass* of `UIViewController`, as illustrated by the code below:

```csharp
public partial class ViewController : UIViewController
{
    public ViewController (IntPtr handle) : base (handle)
    {

    }
}
```

The `ViewController` now drives the interactions of the Content View Hierarchy associated with this View Controller in the Storyboard. Next you’ll learn about the View Controller's role in managing the Views by introducing a process called the View lifecycle.

> [!NOTE]
> For visual-only screens that don’t require user interaction, the **Class** property can be left blank in the **Properties Pad**. This sets the View Controller's backing class as the default implementation of a `UIViewController`, which is appropriate if you don’t plan on adding custom code.

### View lifecycle

The View Controller is in charge of loading and unloading Content View Hierarchies from the Window. When something of importance happens to a View in the Content View Hierarchy, the operating system notifies the View Controller through events in the View lifecycle. By overriding methods in the View lifecycle, you can interact with the objects on the screen and create a dynamic, responsive user interface.

These are the basic lifecycle methods and their function:

-  **ViewDidLoad** - Called  *once* the first time the View Controller loads its Content View Hierarchy into memory. This is a good place to do initial setup because it is when Subviews first become available in code.
-  **ViewWillAppear** - Called every time a View Controller's View is about to be added to a Content View Hierarchy and appear on the screen.
-  **ViewWillDisappear** - Called every time a View Controller's View is about to be removed from a Content View Hierarchy and disappear from the screen. This lifecycle event is used for cleanup and saving state.
-  **ViewDidAppear** and  **ViewDidDisappear** - Called when a View gets added or removed from the Content View Hierarchy, respectively.


When custom code is added to any stage of the Lifecycle, that Lifecycle method’s *base implementation* must be *overriden*. This is achieved by tapping into the existing Lifecycle method, which has some code already attached to it, and  extending it with additional code. The base implementation is called from inside the method to make sure the original code runs before the new code. An example of this is demonstrated in the next section.

For more information on working with View Controllers, refer to Apple's [View Controller Programming Guide for iOS](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1) and the [UIViewController reference](https://developer.apple.com/documentation/uikit/uiviewcontroller?language=objc).

### Responding to user interaction

The most important role of the View Controller is responding to user interaction, such as button presses, navigation, and more. The simplest way to handle user interaction is to wire up a control to listen to user input and attach an event handler to respond to the input. For example, a button could be wired up to respond to a touch event, as demonstrated in the Phoneword app.

Now that there is have a deeper understanding of Views and View Controllers, let's explore how this works.
In the `Phoneword_iOS` Project, a button was added called `TranslateButton` to the Content View Hierarchy:

 [![](hello-ios-deepdive-images/image1.png "A button was added called TranslateButton to the Content View Hierarchy")](hello-ios-deepdive-images/image1.png#lightbox)

When a **Name** is assigned to the **Button** control in the **Properties Pad**, the iOS designer automatically mapped it to a control in the **ViewController.designer.cs**, making the `TranslateButton` available inside the `ViewController` class. Controls first become available in the `ViewDidLoad` stage of the View Lifecycle, so this lifecycle method is used to respond to the user's touch:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // wire up TranslateButton here
}
```

The Phoneword app uses a touch event called `TouchUpInside` to listen to the user's touch. `TouchUpInside` listens for a touch up event (finger lifting off the screen) that follows a touch down (finger touching the screen) inside the bounds of the control. The opposite of `TouchUpInside` is the `TouchDown` event, which fires when the user presses down on a control. The `TouchDown` event captures a lot of noise and gives the user no option to cancel the touch by sliding their finger off the control. `TouchUpInside` is the most common way to respond to a **Button** touch and creates the experience the user expects when pressing a button. More information on this is available in Apple’s [iOS Human Interface Guidelines](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html).

The app handled the `TouchUpInside` event with a lambda, but a delegate or a named event handler could have also been used. The final Button code resembled the following:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
      translatedNumber = Core.PhonewordTranslator.ToNumber(PhoneNumberText.Text);
      PhoneNumberText.ResignFirstResponder ();

      if (translatedNumber == "") {
        CallButton.SetTitle ("Call", UIControlState.Normal);
        CallButton.Enabled = false;
      } else {
        CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
        CallButton.Enabled = true;
      }
  };
}
```

## Additional concepts introduced in Phoneword

The Phoneword application introduced several concepts not covered in this guide. These concepts include:

- **Change Button Text** – The Phoneword app demonstrated how to change the text of a **Button** by calling `SetTitle` on the **Button** and passing in the new text and the **Button**’s _Control State_. For example, the following code changes the CallButton’s text to “Call”:

    ```csharp
    CallButton.SetTitle ("Call", UIControlState.Normal);
    ```
- **Enable and Disable Buttons** – **Buttons** can be in an `Enabled` or `Disabled` state. A disabled **Button** won’t respond to user input. For example, the following code disables the `CallButton`:
      CallButton.Enabled = false;
  For more information on buttons, refer to the [Buttons](~/ios/user-interface/controls/buttons.md) guide.
- **Dismiss the Keyboard** – When the user taps the Text Field, iOS displays the keyboard to let the user enter input. Unfortunately, there is no built-in functionality to dismiss the keyboard. The following code is added to the `TranslateButton` to dismiss the keyboard when the user presses the `TranslateButton`:
      PhoneNumberText.ResignFirstResponder ();
  For another example of dismissing the keyboard, refer to the [Dismiss the Keyboard](https://github.com/xamarin/recipes/tree/master/Recipes/ios/input/keyboards/dismiss_the_keyboard) recipe.
- **Place Phone Call with URL** – In the Phoneword app, an Apple URL scheme is used to launch the system phone app. The custom URL scheme consists of a “tel:” prefix and the translated phone number, as illustrated by the code below:

    ```csharp
    var url = new NSUrl ("tel:" + translatedNumber);
    if (!UIApplication.SharedApplication.OpenUrl (url))
    {
        // show alert Controller
    }
    ```
- **Show an Alert** – When a user tries to place a phone call on a device that doesn’t support calls – for example the Simulator or an iPod Touch – an alert dialog is displayed to let the user know the phone call can’t be placed. The code below creates and populates an alert controller:

    ```csharp
    if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
    ```

For more information on iOS alert views, refer to the [Alert Controller recipe](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller).

## Testing, deployment, and finishing touches

Both Visual Studio for Mac and Visual Studio provide many options for testing and deploying an application. This section covers debugging options, demonstrates testing applications on device, and introduces tools for creating custom app icons and launch images.

### Debugging tools

Sometimes issues in application code are difficult to diagnose. To help diagnose complex code issues, you could [Set a Breakpoint](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [Step Through Code](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code), or [Output Information to the Log Window](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

### Deploy to a device

The iOS Simulator is a quick way to test an application. The Simulator has a number of useful optimizations for testing, including mock location, [simulating movement](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/test_location_changes_in_simulator), and more. However, users will not consume the final app in a Simulator. All applications should be tested on real devices early and often.

A device takes time to provision and requires an Apple Developer Account. The [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) guide gives thorough instructions on getting a device ready for development.

> [!NOTE]
> At present, due to a requirement from Apple, it is necessary to have a a development certificate or _signing identity_ to build you code for device or simulator. Follow the steps in the [Device Provisioning guide](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) to set this up.

Once the device is provisioned, you can deploy to it by plugging it in, changing the target in the build toolbar to the iOS Device, and pressing **Start** ( **Play**) as illustrated by the following screenshot:

# [Visual Studio for Mac](#tab/vsmac)

![](hello-ios-deepdive-images/image46new.png "Pressing Start/Play")

# [Visual Studio](#tab/vswin)

![](hello-ios-deepdive-images/vs-image46.png "Pressing Start/Play")

-----

The app will deploy to the iOS device:

[![](hello-ios-deepdive-images/image1.png "The app will deploy to the iOS device and run")](hello-ios-deepdive-images/image1.png#lightbox)

### Generate custom icons and launch images

Not everyone has a designer available to create the custom icons and launch images an app needs to stand out. Here are several alternate approaches to generating custom app artwork:

# [Visual Studio for Mac](#tab/vsmac)

- [**Sketch**](https://www.sketchapp.com") – Sketch is a Mac app for designing user interfaces, icons, and more. This is the app that was used to design the Xamarin App Icons and Launch Images set. Sketch 3 is available on the App Store. You can try out the free [Sketch Tool](http://bohemiancoding.com/sketch/tool/) as well.
- [**Pixelmator**](http://www.pixelmator.com/) – A versatile image editing app for Mac that costs about $30.
- [**Glyphish**](http://www.glyphish.com/) – High-quality prebuilt icon sets for free download and purchase.
- [**Fiverr**](http://www.fiverr.com/) – Choose from a variety of designers to create an icon set for you, starting at $5.  Can be hit or miss but a good resource if you need icons designed on the fly

# [Visual Studio](#tab/vswin)

* Visual Studio – You can use this to create a simple icon set for your app directly in the IDE.
- [**Glyphish**](http://www.glyphish.com/) – High-quality prebuilt icon sets for free download and purchase.
- [**Fiverr**](http://www.fiverr.com/) – Choose from a variety of designers to create an icon set for you, starting at $5.  Can be hit or miss but a good resource if you need icons designed on the fly

-----

For more information about icon and launch image sizes and requirements, refer to the [Working with Images guide](~/ios/app-fundamentals/images-icons/index.md).

## Summary

Congratulations! You now have a solid understanding of the components of a Xamarin.iOS application as well as the tools used to create them.
In the [next tutorial in the Getting Started series](~/ios/get-started/hello-ios-multiscreen/index.md), you’ll extend our application to handle multiple screens. Along the way you’ll implement a Navigation Controller, learn about Storyboard Segues, and introduce the Model, View, Controller (MVC) pattern as you extend our application to handle multiple screens.


## Related links

- [Hello, iOS (sample)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines/ios/overview/themes/)
- [iOS Provisioning Portal](http://developer.apple.com/account/#/overview)
