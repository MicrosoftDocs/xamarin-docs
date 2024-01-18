---
title: "Siri Shortcuts in Xamarin.iOS"
description: "This document describes how to use Siri Shortcuts in iOS 12. It discusses NSUserActivities, custom intents, assigning voice shortcuts, relevant shortcuts, and more."
ms.service: xamarin
ms.assetid: 86424F79-3A7D-436E-927D-9A3267DA333B
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/08/2018
no-loc: [Objective-C]
---
# Siri Shortcuts in Xamarin.iOS

In [iOS 10](~/ios/platform/sirikit/index.md), Apple introduced SiriKit,
making it possible to build messaging, VoIP calling, payments, workouts,
ride booking, and photo search apps that interact with Siri.

In [iOS 11](~/ios/platform/introduction-to-ios11/sirikit.md), SiriKit
gained support for more types of apps and greater flexibility for UI
customization.

iOS 12 adds Siri Shortcuts, allowing all types of apps to expose their
functionality to Siri. Siri learns when certain app-based tasks are most
relevant to the user and uses this knowledge to suggest potential actions
via _shortcuts_. Tapping on a shortcut or invoking it with a voice
command will open an app or run a background task.

Shortcuts should be used to accelerate a user's ability to accomplish a
common task – in many cases without even opening the app in question.

## Sample app: Soup Chef

To better understand Siri Shortcuts, take a look at the [Soup Chef](/samples/xamarin/ios-samples/ios12-soupchef)
sample app. Soup Chef allows users to place orders from an imaginary soup
restaurant, view their order history, and define phrases to use when
ordering soup by interacting with Siri.

> [!TIP]
> Before testing Soup Chef on an iOS 12 simulator or device, enable the
> following two settings, which are useful when debugging shortcuts:
>
> - In the **Settings** app, enable **Developer > Display Recent Shortcuts**.
> - In the **Settings** app, enable **Developer > Display Donations on Lock Screen**.
>
> These debug settings make it easy to find recently-created (instead of
> predicted) shortcuts on the iOS lock screen and search screen.

To use the sample app:

- Install and run the Soup Chef sample app on an iOS 12 simulator or
[device](#testing-on-device).
- Click the **+** button in the upper right corner to create a new order.
- Select a type of soup, specify a quantity and options, and tap **Place Order**.
- On the **Order History** screen, tap the newly created order to view its
details.
- At the bottom of order details screen, tap **Add to Siri**.
- Record a voice phrase to associate with the order and tap **Done**.
- Minimize Soup Chef, invoke Siri, and place the order again by using the
voice phrase you recorded.
- After Siri completes the order, reopen Soup Chef and notice that the new
order is listed on the **Order History** screen.

The sample app demonstrates how to:

- [Use an NSUserActivity shortcut to open an app](#using-an-nsuseractivity-shortcut-to-open-an-app).
- [Use a custom intent shortcut to perform a task](#using-a-custom-intent-shortcut-to-perform-a-task).
- [Provide a user interface for a custom intent](#providing-a-user-interface-for-a-custom-intent).
- [Create a voice shortcut](#creating-a-voice-shortcut).

## Info.plist and Entitlements.plist

Before digging more deeply in to the Soup Chef code, take a look at its
**Info.plist** and **Entitlements.plist** files.

### Info.plist

The **Info.plist** file in the **SoupChef** project defines the **Bundle
Identifier** as `com.xamarin.SoupChef`. This bundle identifier will be
used as a prefix for the bundle identifiers of the Intents and Intents UI
extensions discussed later in this document.

The **Info.plist** file also contains the following entry:

```xml
<key>NSUserActivityTypes</key>
<array>
    <string>OrderSoupIntent</string>
    <string>com.xamarin.SoupChef.viewMenu</string>
</array>
```

This `NSUserActivityTypes` key/value pair indicates that Soup Chef knows
how to handle an `OrderSoupIntent`, and an [`NSUserActivity`](xref:Foundation.NSUserActivity)
having an [`ActivityType`](xref:Foundation.NSUserActivity.ActivityType)
of "com.xamarin.SoupChef.viewMenu".

Activities and custom intents passed to the app itself, as opposed to its
extensions, are handled in the `AppDelegate`
(a [`UIApplicationDelegate`](xref:UIKit.UIApplicationDelegate)
by the [`ContinueUserActivity`](xref:UIKit.UIApplicationDelegate.ContinueUserActivity*)
method.

### Entitlements.plist

The **Entitlements.plist** file in the **SoupChef** project contains the
following entries:

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
<key>com.apple.developer.siri</key>
<true/>
```

This configuration indicates that the app uses the
"group.com.xamarin.SoupChef" app group. The **SoupChefIntents** app
extension uses this same app group, which allows the two projects to share
[`NSUserDefaults`](xref:Foundation.NSUserDefaults)
data.

The `com.apple.developer.siri` key indicates that the app interacts with
Siri.

> [!NOTE]
> The **SoupChef** project's build configuration sets **Custom Entitlements**
> to **Entitlements.plist**.

## Using an NSUserActivity shortcut to open an app

To create a shortcut that opens an app to display specific content,
create an `NSUserActivity` and attach it to the view controller for the
screen you'd like the shortcut to open.

### Setting up an NSUserActivity

On the menu screen, `SoupMenuViewController` creates an `NSUserActivity`
and assigns it to the view controller's [`UserActivity`](xref:UIKit.UIResponder.UserActivity)
property:

```csharp
public override void ViewDidLoad()
{
    base.ViewDidLoad();
    UserActivity = NSUserActivityHelper.ViewMenuActivity;
}
```

Setting the `UserActivity` property _donates_ the activity to Siri. From
this donation, Siri gains information about when and where this activity is
relevant to the user and learns to better suggest it in the future.

`NSUserActivityHelper` is a utility class included in the **SoupChef**
solution, in the **SoupKit** class library. It creates an `NSUserActivity`
and sets various properties related to Siri and search:

```csharp
public static string ViewMenuActivityType = "com.xamarin.SoupChef.viewMenu";

public static NSUserActivity ViewMenuActivity {
    get
    {
        var userActivity = new NSUserActivity(ViewMenuActivityType)
        {
            Title = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_TITLE", "View menu activity title"),
            EligibleForSearch = true,
            EligibleForPrediction = true
        };

        var attributes = new CSSearchableItemAttributeSet(NSUserActivityHelper.SearchableItemContentType)
        {
            ThumbnailData = UIImage.FromBundle("tomato").AsPNG(),
            Keywords = ViewMenuSearchableKeywords,
            DisplayName = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_TITLE", "View menu activity title"),
            ContentDescription = NSBundleHelper.SoupKitBundle.GetLocalizedString("VIEW_MENU_CONTENT_DESCRIPTION", "View menu content description")
        };
        userActivity.ContentAttributeSet = attributes;

        var phrase = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_LUNCH_SUGGESTED_PHRASE", "Voice shortcut suggested phrase");
        userActivity.SuggestedInvocationPhrase = phrase;
        return userActivity;
    }
}
```

Note the following features in particular:

- Setting `EligibleForPrediction` to `true` indicates that Siri can
predict this activity and surface it as a shortcut.
- The [`ContentAttributeSet`](xref:Foundation.NSUserActivity.ContentAttributeSet)
array is a standard [`CSSearchableItemAttributeSet`](xref:CoreSpotlight.CSSearchableItemAttributeSet)
used to include an `NSUserActivity` in iOS search results.
- [`SuggestedInvocationPhrase`](xref:Foundation.NSUserActivity.SuggestedInvocationPhrase)
is a phrase that Siri will suggest to the user as a potential choice when
assigning a phrase to a shortcut.

### Handling an NSUserActivity shortcut

To handle an `NSUserActivity` shortcut invoked by a user, an iOS
application must override the `ContinueUserActivity` method of the
`AppDelegate` class, responding based on the `ActivityType` field of the
passed-in `NSUserActivity` object:

```csharp
public override bool ContinueUserActivity(UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    // ...
    else if (userActivity.ActivityType == NSUserActivityHelper.ViewMenuActivityType)
    {
        HandleUserActivity();
        return true;
    }
    // ...
}
```

This method calls `HandleUserActivity`, which finds the segue to the
menu screen and invokes it:

```csharp
void HandleUserActivity()
{
    var rootViewController = Window?.RootViewController as UINavigationController;
    var orderHistoryViewController = rootViewController?.ViewControllers?.FirstOrDefault() as OrderHistoryTableViewController;
    if (orderHistoryViewController is null)
    {
        Console.WriteLine("Failed to access OrderHistoryTableViewController.");
        return;
    }
    var segue = OrderHistoryTableViewController.SegueIdentifiers.SoupMenu;
    orderHistoryViewController.PerformSegue(segue, null);
}
```

### Assigning a phrase to an NSUserActivity

To assign a phrase to an `NSUserActivity`, open the iOS **Settings** app
and choose **Siri & Search > My Shortcuts**. Then, select the shortcut
(in this case, "Order Lunch") and record a phrase.

Invoking Siri and using this phrase will open Soup Chef to the menu
screen.

## Using a custom intent shortcut to perform a task

### Defining a custom intent

To provide a shortcut that allows a user to quickly complete a specific task
related to your app, create a custom intent. A custom intent represents a
task a user may want to complete, parameters relevant to that task, and
potential responses resulting from the task's execution. Depending on how
a custom intent is defined, invoking it can either open your app or run a
background task.

Use Xcode 10 to create custom intents. In the
[SoupChef repository](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef),
the custom intent is defined in **OrderSoupIntentCodeGen**, an
Objective-C project. Open this project and select
the **Intents.intentdefinition** file in the **Project Navigator** to view
the **OrderSoup** intent.

Notice the following features:

- The intent has a **Category** of **Order**. There are various pre-defined
categories that can be used for custom intents; select the one that most
closely matches the task your custom intent will enable. Since this solution 
is a soup ordering app, **OrderSoupIntent** uses **Order**.
- The **Confirmation** checkbox indicates whether or not Siri must request
confirmation before executing the task. For the **Order Soup**
intent in Soup Chef, this option is enabled since the user is making
a purchase.
- The **Parameters** section of the **.intentdefinition** file defines
the parameters relevant to a shortcut. To place a soup order, Soup Chef
must know the type of soup, its quantity, and any associated options.
Each parameter has a type; parameter that cannot be represented by a
predefined type are set as **Custom**.
- The **Shortcut Types** interface describes the various parameter
combinations Siri can use when suggesting your shortcut. The associated
**Title** and **Subtitle** sections allow you to define the messages Siri
will use when presenting a suggested shortcut to the user.
- The **Supports Background Execution** checkbox should be selected for
any shortcut that can be executed without opening the app for further user
interaction.

### Defining custom intent responses

The **Response** item nested below the **OrderSoup** intent represents the
potential responses that result from a soup order.

In the **OrderSoup** intent's response definition, note the following features:

- A response's **Properties** can be used to customize the message
communicated back to the user. The **OrderSoup** intent response has
**soup** and **waitTime** properties.
- The **Response Templates** specify the various success and failure
messages that can be used to indicate status after an intent's task has
completed.
- The **Success** checkbox should be selected for responses that indicate
 success.
- The **OrderSoupIntent** success response uses the **soup** and
 **waitTime** properties to provide a friendly and useful message
 describing when the soup order will be ready.

### Generating code for the custom intent

Building the Xcode project containing this custom intent definition
causes Xcode to generate code that can be used to interact
programmatically with the custom intent and its responses.

To view this generated code:

- Open **AppDelegate.m**.
- Add an import to the custom intent's header file:
`#import "OrderSoupIntent.h"`
- In any method in the class, add a reference to `OrderSoupIntent`.
- Right-click on `OrderSoupIntent` and choose **Jump to Definition**.
- Right-click in the newly opened file, **OrderSoupIntent.h**, and select
**Show in Finder**.
- This action will open a **Finder** window that contains a **.h** and **.m** file
containing the generated code.

This generated code includes:

- `OrderSoupIntent` – A class that represents the custom intent.
- `OrderSoupIntentHandling` – A protocol that defines the methods that will
be used to confirm that the intent should be executed and the method that
actually executes it.
- `OrderSoupIntentResponseCode` – An enum that defines various response
statuses.
- `OrderSoupIntentResponse` – a class that represents the response to an
intent's execution.

### Creating a binding to the custom intent

To use the code generated by Xcode in a Xamarin.iOS app, create a C#
binding for it.

#### Creating a static library and C# binding definitions

In the [SoupChef repository](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef),
take a look in the **OrderSoupIntentStaticLib** folder, and open the
**OrderSoupIntentStaticLib.xcodeproj** Xcode project.

This **Cocoa Touch Static Library** project contains the
**OrderSoupIntent.h** and **OrderSoupIntent.m** files generated by
Xcode.

#### Configuring the static library project build settings

In the Xcode **Project Navigator**, select the top-level project,
**OrderSoupIntentStaticLib**, and navigate to **Build Phases > Compile Sources**.
Notice that **OrderSoupIntent.m** (which imports **OrderSoupIntent.h**) is
listed here. In **Link Binary With Libraries**, notice that
**Intents.framework** and **Foundation.framework** are included.
With these settings in place, the framework will build correctly.

#### Building the static library and generating C# bindings definitions

To build the static library and generate C# bindings definitions for it,
follow these steps:

- [Install Objective Sharpie](../../../cross-platform/macios/binding/objective-sharpie/get-started.md?context=xamarin%252fmac#installing-objective-sharpie),
the tool used to generate bindings definitions from the **.h** and **.m** files
created by Xcode.

- Configure your system to use Xcode 10 **Command Line Tools**:

  > [!WARNING]
  > Updating the selected **Command Line Tools** impacts all installed
  > versions of Xcode on your system. When you are done using the Soup
  > Chef sample app, be sure to revert this setting to its original
  > configuration.

  - In Xcode, choose **Xcode > Preferences > Locations** and set
  **Command Line Tools** to the most current Xcode 10 installation
  available on your system.

- In the terminal, `cd` to the **OrderSoupIntentStaticLib** directory.

- Type `make`, which builds:

  - The static library, **libOrderSoupIntentStaticLib.a**
  - In the **bo** output directory, C# bindings definitions:
    - **ApiDefinitions.cs**
    - **StructsAndEnums.cs**

The **OrderSoupIntentBindings** project, which relies on this static library
and its associated bindings definitions, builds these items automatically.
However, manually running through the above process will ensure that it
builds as expected.

For more information about creating a static library and using Objective
Sharpie to create C# bindings definitions, take a look at the
[Binding an iOS Objective-C library](../binding-objective-c/walkthrough.md?tabs=vsmac#creating-a-static-library)
walkthrough.

#### Creating a bindings library

With the static library and the C# bindings definitions created, the
remaining piece necessary to consume the Xcode-generated intent-related code
in a Xamarin.iOS project is a bindings library.

In the [Soup Chef repository](https://github.com/xamarin/ios-samples/tree/master/ios12/SoupChef),
open the **SoupChef.sln** file. Among other things, this solution
contains **OrderSoupIntentBinding**, a bindings library for the static
library generated earlier.

Notice in particular that this project includes:

- **ApiDefinitions.cs** – A file generated earlier by Objective Sharpie and
added to this project. This file's **Build Action** is set to
**ObjcBindingApiDefinition**.
- **StructsAndEnums.cs** – Another file generated earlier by Objective
Sharpie and added to this project. This file's **Build Action** is set to
**ObjcBindingCoreSource**.
- A **Native Reference** to **libOrderSoupIntentStaticLib.a**, the static
library built earlier. Update the native reference properties and specify the following values:

    1. Frameworks = `Foundation Intents`
    1. Smart Link = `On`
    1. Force Load = `On`
    1. Kind = `Static`

> [!NOTE]
> Both **ApiDefinitions.cs** and **StructsAndEnums.cs** contain attributes
> such as `[Watch (5,0), iOS (12,0)]`. These attributes, generated by
> Objective Sharpie, have been commented out since they aren't necessary for
> this project.

For more information about creating a C# bindings library, take a look at the
[Binding an iOS Objective-C Library](../binding-objective-c/walkthrough.md?tabs=vsmac#create-a-xamarinios-binding-project)
walkthrough.

Notice that the **SoupChef** project contains a reference to
**OrderSoupIntentBinding**, which means that it can now access, in C#, the
classes, interfaces, and enums it contains:

- `OrderSoupIntent`
- `OrderSoupIntentHandling`
- `OrderSoupIntentResponse`
- `OrderSoupIntenseResponseCode`

### Creating a Swift framework

The intent definition native code is generated by Xcode by default using the language of your native project. If you define the **Intents.intentdefinition** file in a Swift project, Xcode will generate you a single Swift file with all the required classes, which you can use to create a Swift framework.

> [!TIP]
> You can select a desired language for the generated intent code in the Xcode build settings. Go to **Intent target > Build Settings > Intent Definition Compiler - Code Generation** and select either Swift or Objective-C. You can also keep it automatic to match your target language.

The process of creating a Swift framework is similar to the one described earlier:

1. Create a new Swift framework project.
1. Copy the automatically generated Swift file with intent code to this project, you can find this file as described [here](#generating-code-for-the-custom-intent).
1. Enable the [Objective-C bridging header](../binding-swift/walkthrough.md#build-a-native-library), so the framework is automatically generated with the required by Objective-C sharpie header file.

Once the framework is built, follow the [same steps](#creating-a-bindings-library) described earlier to create a Xamarin binding. You can read more about creating a binding for a Swift framework [here](../binding-swift/walkthrough.md).

### Adding the intent definition file to your solution

In the C# **SoupChef** solution, the **SoupKit** project contains code
shared between the app and its extensions. The **Intents.intentdefinition**
file has been placed in the **Base.lproj** directory of **SoupKit**, and
it has a **Build Action** of **Content**. The build process copies this
file into the Soup Chef app bundle, where it is required for the app to
function correctly.

### Donating an intent

In order for Siri to suggest a shortcut, it must first understand when
the shortcut is relevant.

To give Siri this understanding, Soup Chef _donates_ an intent to Siri
each time the user places a soup order. Based on this donation – when it
was donated, where it was donated, the parameters it contains – Siri
learns when to suggest the shortcut in the future.

**SoupChef** uses the `SoupOrderDataManager` class to place donations.
When called to place a soup order for a user, the `PlaceOrder` method in
turn calls [`DonateInteraction`](xref:Intents.INInteraction.DonateInteraction*):

```csharp
void DonateInteraction(Order order)
{
    var interaction = new INInteraction(order.Intent, null);
    interaction.Identifier = order.Identifier.ToString();
    interaction.DonateInteraction((error) =>
    {
        // ...
    });
}
```

After fetching an intent, it is wrapped in an
[`INInteraction`](xref:Intents.INInteraction).
The `INInteraction` is given an
[`Identifier`](xref:Intents.INInteraction.Identifier*)
that matches the unique ID of the order (it will be helpful later when
deleting intent donations that are no longer valid). Then, the interaction
is donated to Siri.

The call to the `order.Intent` getter fetches an `OrderSoupIntent` that
represents the order by setting its `Quantity`, `Soup`, `Options`, and
image, and an invocation phrase to use as a suggestion when the user
records a phrase for Siri to associate with the intent:

```csharp
public OrderSoupIntent Intent
{
    get
    {
        var orderSoupIntent = new OrderSoupIntent();
        orderSoupIntent.Quantity = new NSNumber(Quantity);
        orderSoupIntent.Soup = new INObject(MenuItem.ItemNameKey, MenuItem.LocalizedString);

        var image = UIImage.FromBundle(MenuItem.IconImageName);
        if (!(image is null))
        {
            var data = image.AsPNG();
            orderSoupIntent.SetImage(INImage.FromData(data), "soup");
        }

        orderSoupIntent.Options = MenuItemOptions
            .ToArray<MenuItemOption>()
            .Select<MenuItemOption, INObject>(arg => new INObject(arg.Value, arg.LocalizedString))
            .ToArray<INObject>();

        var comment = "Suggested phrase for ordering a specific soup";
        var phrase = NSBundleHelper.SoupKitBundle.GetLocalizedString("ORDER_SOUP_SUGGESTED_PHRASE", comment);
        orderSoupIntent.SuggestedInvocationPhrase = String.Format(phrase, MenuItem.LocalizedString);

        return orderSoupIntent;
    }
}
```

#### Removing invalid donations

It's important to remove donations that are no longer valid so that
Siri does not make unhelpful or confusing shortcut suggestions.

In Soup Chef, the **Configure Menu** screen can be used to mark a menu item
as unavailable. Siri should no longer suggest a shortcut to order the
unavailable menu item, so the `RemoveDonation` method of `SoupMenuManager`
deletes donations for menu items that are no longer available. The app implements 
this functionality by:

- Finding orders associated with the now-unavailable menu item.
- Grabbing their identifiers.
- Deleting interactions that have the same identifiers.

```csharp
void RemoveDonation(MenuItem menuItem)
{
    if (!menuItem.IsAvailable)
    {
        Order[] orderHistory = OrderManager?.OrderHistory.ToArray<Order>();
        if (orderHistory is null)
        {
            return;
        }

        string[] orderIdentifiersToRemove = orderHistory
            .Where<Order>((order) => order.MenuItem.ItemNameKey == menuItem.ItemNameKey)
            .Select<Order, string>((order) => order.Identifier.ToString())
            .ToArray<string>();

        INInteraction.DeleteInteractions(orderIdentifiersToRemove, (error) =>
        {
            if (!(error is null))
            {
                Console.WriteLine($"Failed to delete interactions with error {error.ToString()}");
            }
            else
            {
                Console.WriteLine("Successfully deleted interactions");
            }
        });
    }
}
```

#### Validating successful donations

The solution includes multiple projects and specific configuration. In some cases, the application can crash because of incomplete configuration, in other cases, it can silently fail to donate an interaction. It's important to validate successful donations and iOS Developer settings helps with it. Navigate to **Settings > Developer** and enable the following developer options to see recent donations and shortcuts:

- Display Recent Shortcuts
- Display Donations on Lock Screen

Once enabled, every successful donation will appear on the lock screen and below the Siri suggestions options. If after running your application you don't see the donations there, review the following troubleshooting cases:

1. An app fails to create the `OrderSoupIntent` with the following error:

    > Could not create an native instance of the type 'NativeLibrary.OrderSoupIntent': the native class hasn't been loaded.

    This error means that Xamarin is unable to load the native class via the Xamarin binding. To fix that, verify that the native library includes the required code, referenced by the binding project, and proper flags are set, as described [here](#creating-a-bindings-library), set the `Force Load` flag to `On`.

1. An app fails to initialize the loaded native instance of the intent class with the following error:

    > Could not initialize an instance of the type 'NativeLibrary.OrderSoupIntent': the native 'init' method returned nil.

    The issue is related to the missing intent definition file. The Xamarin app should include the original intent definition file with the `Content` type, as described [here](#adding-the-intent-definition-file-to-your-solution).

1. An app creates the intent and calls the donate method without a crash but console output shows a warning about unknown intent type and no donation is made:

    > Cannot donate interaction with OrderSoupIntent that has no valid shortcut types

    To fix the issue the intent has to be properly [defined in the plist](#infoplist), [Siri entitlement has to be enabled](#entitlementsplist) and selected for current build configuration via project settings.

    The **info.plist** of the app:

    ```plist
    <key>NSUserActivityTypes</key>
    <array>
        <string>ScheduleMeetingIntent</string>
    </array>
    ```

    The **Entitlements.plist** of the app with the Siri capability:

    ```plist
    <key>com.apple.developer.siri</key>
    <true/>
    ```

    Custom entitlements should be selected for the targeted build configuration. Go to **Project settings > Build > iOS Bundle Signing** and set **Custom Entitlements** to the **Entitlements.plist** file containing the required entitlements.

### Creating an Intents extension

The code that runs when Siri invokes an intent is placed in an Intents
extension, which can be added as a new project to the same solution as an
existing Xamarin.iOS app such as Soup Chef. In the **SoupChef** solution,
the extension is called **SoupChefIntents**.

#### SoupChefIntents – Info.plist and Entitlements.plist

##### SoupChefIntents – Info.plist

The **Info.plist** in the **SoupChefIntents** project defines the
**Bundle Identifier** as `com.xamarin.SoupChef.SoupChefIntents`.

The **Info.plist** file also contains the following entry:

```xml
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>IntentsRestrictedWhileLocked</key>
        <array/>
        <key>IntentsSupported</key>
        <array>
            <string>OrderSoupIntent</string>
        </array>
        <key>IntentsRestrictedWhileProtectedDataUnavailable</key>
        <array/>
    </dict>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.intents-service</string>
    <key>NSExtensionPrincipalClass</key>
    <string>IntentHandler</string>
</dict>
```

In the above **Info.plist**:

- `IntentsRestrictedWhileLocked` lists intents to be handled
when the device is unlocked.
- `IntentsSupported` lists the intents handled by this extension.
- `NSExtensionPointIdentifier` specifies the type of app extension. For more information, see
[Apple's documentation](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15).
- `NSExtensionPrincipalClass` specifies the class that should be used
to handle intents supported by this extension.

##### SoupChefIntents – Entitlements.plist

The **Entitlements.plist** in the **SoupChefIntents** project has the
**App Groups** capability. This capability is configured to use the same
app group as the **SoupChef** project:

```xml
<key>com.apple.security.application-groups</key>
<array>
    <string>group.com.xamarin.SoupChef</string>
</array>
```

Soup Chef persists data with `NSUserDefaults`. In order to share data
between the app and the app extension, they reference the same app group
in their **Entitlements.plist** files.

> [!NOTE]
> The **SoupChefIntents** project's build configuration sets
> **Custom Entitlements** to **Entitlements.plist**.

#### Handling an OrderSoupIntent background task

An Intents extension executes the necessary background tasks for a shortcut
based on a custom intent.

Siri calls the [`GetHandler`](xref:Intents.INExtension.GetHandler*)
method of the `IntentHandler` class (defined in **Info.plist** as the
`NSExtensionPrincipalClass`) to get an instance of a class that extends
`OrderSoupIntentHandling`, which can be used to handle an `OrderSoupIntent`:

```csharp
[Register("IntentHandler")]
public class IntentHandler : INExtension
{
    public override NSObject GetHandler(INIntent intent)
    {
        if (intent is OrderSoupIntent)
        {
            return new OrderSoupIntentHandler();
        }
        throw new Exception("Unhandled intent type: ${intent}");
    }

    protected IntentHandler(IntPtr handle) : base(handle) { }
}
```

`OrderSoupIntentHandler`, defined in the **SoupKit** project of shared code,
implements two important methods:

- `ConfirmOrderSoup` – Confirms whether or not the task associated with
the intent should actually be executed
- `HandleOrderSoup` – Places the soup order and responds to the user by
calling the passed-in completion handler

#### Handling an OrderSoupIntent that opens the app

An app must properly handle intents that do not run in the background.
These intents are handled in the same way as `NSUserActivity` shortcuts, in the
`ContinueUserActivity` method of `AppDelegate`:

```csharp
public override bool ContinueUserActivity(UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    var intent = userActivity.GetInteraction()?.Intent as OrderSoupIntent;
    if (!(intent is null))
    {
        HandleIntent(intent);
        return true;
    }
    // ...
}  
```

## Providing a user interface for a custom intent

An Intents UI extension provides a custom user interface for an Intents
extension. In the **SoupChef** solution, **SoupChefIntentsUI** is an
Intents UI extension that provides an interface for **SoupChefIntents**.

### SoupChefIntentsUI – Info.plist and Entitlements.plist

#### SoupChefIntentsUI – Info.plist

The **Info.plist** in the **SoupChefIntentsUI** project defines the
**Bundle Identifier** as `com.xamarin.SoupChef.SoupChefIntentsui`.

The **Info.plist** file also contains the following entry:

```xml
<key>NSExtension</key>
<dict>
    <key>NSExtensionAttributes</key>
    <dict>
        <key>IntentsSupported</key>
        <array>
            <string>OrderSoupIntent</string>
        </array>
        <!-- ... -->
    </dict>
    <key>NSExtensionPointIdentifier</key>
    <string>com.apple.intents-ui-service</string>
    <key>NSExtensionMainStoryboard</key>
    <string>MainInterface</string>
</dict>
```

In the above **Info.plist**:

- `IntentsSupported` indicates that the `OrderSoupIntent` is handled by
this Intents UI extension.
- `NSExtensionPointIdentifier` specifies the type of app extension. For more information, see
[Apple's documentation](https://developer.apple.com/library/archive/documentation/General/Reference/InfoPlistKeyReference/Articles/AppExtensionKeys.html#//apple_ref/doc/uid/TP40014212-SW15).
- `NSExtensionMainStoryboard` specifies the storyboard that defines the
primary interface of this extension

#### SoupChefIntentsUI – Entitlements.plist

The **SoupChefIntentsUI** project does not need an **Entitlements.plist**
file.

### Creating the user interface

Since the **Info.plist** for **SoupChefIntentsUI** sets the
`NSExtensionMainStoryboard` key to `MainInterface`, the
**MainInterace.storyboard** file defines the interface for the Intents UI
extension.

In this storyboard, there is a single view controller, of type
**IntentViewController**. It references two views:

- **invoiceView**, of type `InvoiceView`
- **confirmationView**, of type `ConfirmOrderView`

> [!NOTE]
> The interfaces for **invoiceView** and **confirmationView** are defined
> in **Main.storyboard** as secondary views. Visual Studio for Mac and 
> Visual Studio 2017 does not provide support for viewing or editing 
> secondary views; to do so, open **Main.storyboard**
> in Xcode's Interface Builder.

`IntentViewController` implements the
[`IINUIHostedViewControlling`](xref:IntentsUI.IINUIHostedViewControlling)
interface, used to provide a custom interface when working with Siri
Intents. The
[`ConfigureView`](xref:IntentsUI.INUIHostedViewControlling_Extensions.ConfigureView*)
method is called to customize the interface, displaying the confirmation or
the invoice, depending on whether the interaction is being confirmed
([`INIntentHandlingStatus.Ready`](xref:Intents.INIntentHandlingStatus))
or has been executed successfully
([`INIntentHandlingStatus.Success`](xref:Intents.INIntentHandlingStatus)):

```csharp
[Export("configureViewForParameters:ofInteraction:interactiveBehavior:context:completion:")]
public void ConfigureView(
    NSSet<INParameter> parameters,
    INInteraction interaction,
    INUIInteractiveBehavior interactiveBehavior,
    INUIHostedViewContext context,
    INUIHostedViewControllingConfigureViewHandler completion)
{
    // ...
    if (interaction.IntentHandlingStatus == INIntentHandlingStatus.Ready)
    {
        desiredSize = DisplayInvoice(order, intent);
    }
    else if(interaction.IntentHandlingStatus == INIntentHandlingStatus.Success)
    {
        var response = interaction.IntentResponse as OrderSoupIntentResponse;
        if (!(response is null))
        {
            desiredSize = DisplayOrderConfirmation(order, intent, response);
        }
    }
    completion(true, parameters, desiredSize);
}
```

> [!TIP]
> For more information about the `ConfigureView` method, watch Apple's
> WWDC 2017 presentation, [What's New in SiriKit](https://developer.apple.com/videos/play/wwdc2017/214/).

## Creating a voice shortcut

Soup Chef provides an interface to assign a voice shortcut to each order,
making it possible to order soup with Siri. In fact, the interface
used to record and assign voice shortcuts is provided by iOS and requires
little custom code.

In `OrderDetailViewController`, when a user taps the table's **Add to Siri**
row, the [`RowSelected`](xref:UIKit.UITableViewSource.RowSelected*)
method presents a screen to either add or edit a voice shortcut:

```csharp
public override void RowSelected(UITableView tableView, NSIndexPath indexPath)
{
    // ...
    else if (TableConfiguration.Sections[indexPath.Section].Type == OrderDetailTableConfiguration.SectionType.VoiceShortcut)
    {
        INVoiceShortcut existingShortcut = VoiceShortcutDataManager?.VoiceShortcutForOrder(Order);
        if (!(existingShortcut is null))
        {
            var editVoiceShortcutViewController = new INUIEditVoiceShortcutViewController(existingShortcut);
            editVoiceShortcutViewController.Delegate = this;
            PresentViewController(editVoiceShortcutViewController, true, null);
        }
        else
        {
            // Since the app isn't yet managing a voice shortcut for
            // this order, present the add view controller
            INShortcut newShortcut = new INShortcut(Order.Intent);
            if (!(newShortcut is null))
            {
                var addVoiceShortcutVC = new INUIAddVoiceShortcutViewController(newShortcut);
                addVoiceShortcutVC.Delegate = this;
                PresentViewController(addVoiceShortcutVC, true, null);
            }
        }
    }
}
```

Based on whether or not an existing voice shortcut exists for the
currently displayed order, `RowSelected` presents a view controller of
type [`INUIEditVoiceShortcutViewController`](xref:IntentsUI.INUIEditVoiceShortcutViewController)
or [`INUIAddVoiceShortcutViewController`](xref:IntentsUI.INUIAddVoiceShortcutViewController).
In each case, `OrderDetailViewController` sets itself as the view
controller's `Delegate`, which is why it also implements
[`IINUIAddVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIAddVoiceShortcutViewControllerDelegate)
and [`IINUIEditVoiceShortcutViewControllerDelegate`](xref:IntentsUI.IINUIEditVoiceShortcutViewControllerDelegate).

## Testing on device

To run Soup Chef on a device, follow the instructions in this section. Also read the
[note about automatic provisioning](#automatic-provisioning).

### App Group, App IDs, Provisioning Profiles

In the **Certificates, IDs & Profiles** section of the
[Apple Developer Portal](https://developer.apple.com/), do the following steps:

- Create an app group to share data between the Soup Chef app and its
extensions. For example: **group.com.yourcompanyname.SoupChef**

- Create three App IDs: one for the app itself, one for the Intents
extension, and one for the Intents UI extension. For example:

  - App: **com.yourcompanyname.SoupChef**
    - To this App ID, assign the SiriKit and **App Groups**
      capabilities.

  - Intents extension: **com.yourcompanyname.SoupChef.Intents**
    - To this App ID, assign the **App Groups** capability.

  - Intents UI extension: **com.yourcompanyname.SoupChef.Intentsui**
    - This App ID needs no special capabilities.

- After creating the above App IDs, edit the **App Groups** capability
assigned to the app and the Intents extension, specifying the specific
app group created earlier.

- Create three new development provisioning profiles, one for each of the
new App IDs.

- Download these provisioning profiles and double-click each one to install
it. If Visual Studio for Mac or Visual Studio 2017 is already running,
restart it to make sure it registers the new provisioning profiles.

### Editing Info.plist, Entitlements.plist, and source code

In Visual Studio for Mac or Visual Studio 2017, do the following steps:

- Update the various **Info.plist** files in the solution. Set the app,
Intents extension, and Intents UI extension **Bundle Identifier** to the App
IDs defined earlier:

  - App: **com.yourcompanyname.SoupChef**
  - Intents Extension: **com.yourcompanyname.SoupChef.Intents**
  - Intents UI Extension: **com.yourcompanyname.SoupChef.Intentsui**

- Update the **Entitlements.plist** file for the **SoupChef** project:
  - For the **App Groups** capability, set the group to the new app group
  created earlier (in the above example, it was
  **group.com.yourcompanyname.SoupChef**).
  - Make sure that **SiriKit** is enabled.

- Update the **Entitlements.plist** file for the **SoupChefIntents** project:
  - For the **App Groups** capability, set the group to the new app group
    created earlier (in the above example, it was **group.com.yourcompanyname.SoupChef**).

- Finally, open **NSUserDefaultsHelper.cs**. Set the `AppGroup` variable
to the value of your new app group (for example, set it to
`group.com.yourcompanyname.SoupChef`).

### Configuring the build settings

In Visual Studio for Mac or Visual Studio 2017:

- Open the options/properties for the **SoupChef** project. On the
**iOS Bundle Signing** tab, set **Signing Identity** to automatic and
**Provisioning Profile** to the new app-specific provisioning profile you
created earlier.

- Open the options/properties for the **SoupChefIntents** project. On the
**iOS Bundle Signing** tab, set **Signing Identity** to automatic and
**Provisioning Profile** to the new Intents extension-specific provisioning
profile you created earlier.

- Open the options/properties for the **SoupChefIntentsUI** project. On the
**iOS Bundle Signing** tab, set **Signing Identity** to automatic and
**Provisioning Profile** to the new Intents UI extension-specific provisioning
profile you created earlier.

With these changes in place, the app will run on an iOS device.

### Automatic provisioning

You can use [automatic provisioning](../../get-started/installation/device-provisioning/automatic-provisioning.md)
to accomplish many of these provisioning tasks directly in the IDE.
However, automatic provisioning does not set up app groups. You will need
to manually configure the **Entitlements.plist** files with the name of
the app group you would like to use, visit the Apple Developer Portal to
create the app group, assign that app group to each App ID created by
automatic provisioning, regenerate the provisioning profiles (app, Intents
extension, Intents UI extension) to include the newly created app group,
and download and install them.

## Related links

- [Soup Chef (Xamarin)](/samples/xamarin/ios-samples/ios12-soupchef)
- [Soup Chef (Swift)](https://developer.apple.com/documentation/sirikit/accelerating_app_interactions_with_shortcuts?language=objc)
- [SiriKit (Apple)](https://developer.apple.com/sirikit/)
- [Introduction to Siri Shortcuts – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/211/)
- [Building for Voice with Siri Shortcuts – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/214/)
- [Siri Shortcuts on the Siri Watch Face – WWDC 2018](https://developer.apple.com/videos/play/wwdc2018/217/)
- [What's New in SiriKit – WWDC 2017](https://developer.apple.com/videos/play/wwdc2017/214/)
- [Create an Intents App Extension (Apple)](https://developer.apple.com/documentation/sirikit/creating_an_intents_app_extension?language=objc)
- [Bind an iOS Swift library](../binding-swift/walkthrough.md)
