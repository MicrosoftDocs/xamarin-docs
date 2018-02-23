---
title: "Hello, iOS Multiscreen"
description: "In this two-part guide, we expand the Phoneword application we created in the Hello, iOS guide to handle a second screen. Along the way we’ll introduce the Model-View-Controller design pattern, implement our first iOS navigation, and develop a deeper understanding of iOS application structure and functionality."
ms.topic: article
ms.prod: xamarin
ms.assetid: d72e6230-c9ee-4bee-90ec-877d256821aa
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 12/02/2016
---

# Hello.iOS Multiscreen Quickstart

This part of the walkthrough will add a second screen to the Phoneword application that will display a history of the phone numbers that were called with the app. The final application will have a second screen that displays the call history, as illustrated by the following screenshot:

 [ ![](hello-ios-multiscreen-quickstart-images/00.png "The final application will have a second screen that displays the call history, as illustrated by this screenshot")](hello-ios-multiscreen-quickstart-images/00.png)

The [accompanying Deep Dive](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md), will review the application that is build built and discuss architecture, navigation, and other new iOS concepts that we encounter along the way.

 <a name="Requirements" />

## Requirements

This guide resumes where the Hello, iOS document left off, and requires completion of the [Hello, iOS Quickstart](~/ios/get-started/hello-ios/index.md). The completed version of the Phoneword app can be downloaded from the [Hello, iOS sample](https://developer.xamarin.com/samples/monotouch/Hello_iOS/).

# [Visual Studio for Mac](#tab/vsmac)

## Walkthrough

This walkthrough will add a Call History screen to our **Phoneword** application.


1. Open the **Phoneword** application in Visual Studio for Mac. If necessary, the completed Phoneword application from the [Hello, iOS walkthrough](~/ios/get-started/hello-ios/index.md) guide can be downloaded from [here](https://developer.xamarin.com/samples/monotouch/Hello_iOS/).


2. Open the **Main.storyboard** file from the **Solution Pad**:

  ![](hello-ios-multiscreen-quickstart-images/02new.png "The Main.storyboard in the iOS Designer")


3. Drag a **Navigation Controller** from the **Toolbox** onto the design surface (You might need to zoom out to fit these all on the design surface!):

  ![](hello-ios-multiscreen-quickstart-images/03new.png "Drag a Navigation Controller from the Toolbox onto the design surface")


4. Drag the **Sourceless Segue** (that’s the gray arrow to the left of single View Controller) to the **Navigation Controller** to change the starting point of the application:

  ![](hello-ios-multiscreen-quickstart-images/04new.png "Drag the Sourceless Segue to the Navigation Controller to change the starting point of the application")


5. Select the existing **Root View Controller** by clicking on the bottom bar, and press **Delete** to remove it from the design surface.
Then, move the **Phoneword** Scene next to the **Navigation Controller**:

  ![](hello-ios-multiscreen-quickstart-images/05new.png "Move the Phoneword Scene next to the Navigation Controller")


6. Set the **ViewController** as the Navigation Controller’s **Root View Controller**. Press down the
**Ctrl** key and click inside the **Navigation Controller**. A blue line should appear. Then, still holding down the **Ctrl**
 key, drag from the **Navigation Controller** to the **Phoneword** Scene and release. This is called _Ctrl-dragging_:

 ![](hello-ios-multiscreen-quickstart-images/06.png "Drag from the Navigation Controller to the Phoneword Scene and release")


7. From the popover, set the relationship to **Root**:

  ![](hello-ios-multiscreen-quickstart-images/07new.png "Setting the relationship to Root")

  The **ViewController** is now the **Navigation Controller’s Root View Controller:**

  ![](hello-ios-multiscreen-quickstart-images/08.png "The ViewController is now the Navigation Controllers Root View Controller")


8. Double-click on the **Phoneword** screen’s **Title** bar and change the **Title** to **Phoneword**:

  ![](hello-ios-multiscreen-quickstart-images/09.png "Change the Title to 'Phoneword'")


9. Drag a **Button** from the **Toolbox** and place it under the **Call Button**. Drag the
handles to make the new **Button** the same width as the **Call Button**:

  ![](hello-ios-multiscreen-quickstart-images/10new.png "Make the new Button the same width as the Call Button")


10. In the **Properties Pad**, change the **Name** of the button to **CallHistoryButton**
 and change the **Title** to **Call History**:

  ![](hello-ios-multiscreen-quickstart-images/11new.png "Change the Name of the Button to CallHistoryButton and change the Title to Call History")


11. Create the **Call History** screen. From the **Toolbox**, drag a **Table View Controller**
 onto the design surface:

 ![](hello-ios-multiscreen-quickstart-images/12new.png "Drag a Table View Controller onto the design surface")


12. Next, select the **Table View Controller** by clicking on the black bar at the bottom of the Scene. In the **Properties Pad**,
change the **Table View Controller’s** class to `CallHistoryController` and press **Enter**:

  ![](hello-ios-multiscreen-quickstart-images/13new.png "Change the Table View Controllers class to CallHistoryController")

  The iOS Designer will generate a custom backing class called `CallHistoryController` to manage this screen’s Content View Hierarchy.
  The **CallHistoryController.cs** file will appear in the **Solution Pad**:

  ![](hello-ios-multiscreen-quickstart-images/14new.png "The CallHistoryController.cs file in the Solution Pad")


13. Double-click on the **CallHistoryController.cs** file to open it and replace the contents with the following code:

  ```csharp
  using System;
  using Foundation;
  using UIKit;
  using System.Collections.Generic;

  namespace Phoneword_iOS
  {
      public partial class CallHistoryController : UITableViewController
      {
          public List<string> PhoneNumbers { get; set; }

          static NSString callHistoryCellId = new NSString ("CallHistoryCell");

          public CallHistoryController (IntPtr handle) : base (handle)
          {
              TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
              TableView.Source = new CallHistoryDataSource (this);
              PhoneNumbers = new List<string> ();
          }

          class CallHistoryDataSource : UITableViewSource
          {
              CallHistoryController controller;

              public CallHistoryDataSource (CallHistoryController controller)
              {
                  this.controller = controller;
              }

              public override nint RowsInSection (UITableView tableView, nint section)
              {
                  return controller.PhoneNumbers.Count;
              }

              public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
              {
                  var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                  int row = indexPath.Row;
                  cell.TextLabel.Text = controller.PhoneNumbers [row];
                  return cell;
              }
          }
      }
  }
  ```

  Save the application (**⌘ + s**) and build it (**⌘ + b**) to ensure there are no errors.


14. Create a _Segue_ (transition) between the **Phoneword** Scene and the **Call History** Scene.
  In the **Phoneword Scene**, select the **Call History Button** and Ctrl-drag from the
  **Button** to the **Call History** Scene:

  ![](hello-ios-multiscreen-quickstart-images/15.png "Ctrl-drag from the Button to the Call History Scene")

  From the **Action Segue** popover, select **Show**

  The iOS Designer will add a Segue between the two Scenes:

  ![](hello-ios-multiscreen-quickstart-images/17new.png "The Segue between the two Scenes")


15. Add a **Title** to the **Table View Controller** by selecting the black bar at the bottom of the Scene
and changing the **View Controller Title** to **Call History** in the **Properties Pad**:

  ![](hello-ios-multiscreen-quickstart-images/18new.png "Change the View Controller Title to Call History in the Properties Pad")

16. When the application is run, the **Call History Button** will open the **Call History** screen,
  but the Table View will be empty because there is no code to to keep track of and display the phone numbers.

  This app will store the phone numbers as a list of strings.

  Add  a `using` directive for `System.Collections.Generic`  at the top of **ViewController**:

  ```csharp
  using System.Collections.Generic;
  ```



17. Modify the `ViewController` class with the following code:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the View Controller that’s powering the screen we’re
          // transitioning to

          var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

          //set the Table View Controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryContoller != null)
          {
            callHistoryContoller.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```


There are a few things happening here
  * The variable `translatedNumber` was moved from the `ViewDidLoad` method to a _class-level variable_.
  * The  **CallButton** code was modified to add dialed numbers to the list of phone numbers by calling
`PhoneNumbers.Add(translatedNumber)`
  * The `PrepareForSegue` method was added

  Save and build the application to make sure there are no errors.

20. Press the **Start** button to launch the application inside the **iOS Simulator**:

  ![](hello-ios-multiscreen-quickstart-images/19.png "Press the Start button to launch the application inside the iOS Simulator")


Congratulations on completing your first multi-screen Xamarin.iOS application!

# [Visual Studio](#tab/vswin)

## Walkthrough

This walkthrough will add a Call History screen to our **Phoneword** application.


1. Open the **Phoneword** application in Visual Studio. If necessary, download the [completed Phoneword application](https://developer.xamarin.com/samples/monotouch/Hello_iOS/) from the [Hello, iOS walkthrough](~/ios/get-started/hello-ios/index.md) guide
  download the . Recall that it is necessary to connect to a [Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) to use the iOS Designer, and iOS simulator.


2. Start by editing the user interface. Open the **Main.storyboard** file from the **Solution Explorer**, making sure that **View As** is set to _iPhone 6_:

  ![](hello-ios-multiscreen-quickstart-images/image1.png "The Main.storyboard in the iOS Designer")


3. Drag a **Navigation Controller** from the **Toolbox** onto the design surface:

  ![](hello-ios-multiscreen-quickstart-images/image2.png "Drag a Navigation Controller from the Toolbox onto the design surface")


4. Drag the **Sourceless Segue** (that’s the gray arrow to the left of the **Phoneword** Scene) from the **Phoneword** Scene
  to the **Navigation Controller** to change the starting point of the application:

  ![](hello-ios-multiscreen-quickstart-images/image3.png "Drag the Sourceless Segue to the Navigation Controller to change the starting point of the application")


5. Select the **Root View Controller** by clicking on the black bar, and press **Delete** to remove it from the design surface.
  Then, move the **Phoneword** Scene next to the **Navigation Controller**:

  ![](hello-ios-multiscreen-quickstart-images/image4.png "Move the Phoneword Scene next to the Navigation Controller")


6. Set the **ViewController** as the Navigation Controller’s `Root View Controller`. Press down the
  **Ctrl** key and click inside the **Navigation Controller**. A blue line should appear. Then, still holding down the **Ctrl**
  key, drag from the **Navigation Controller** to the **Phoneword** Scene and release. This is called _Ctrl-dragging_:

  ![](hello-ios-multiscreen-quickstart-images/image5.png "Drag from the Navigation Controller to the Phoneword Scene and release")


7. From the popover, set the relationship to **Root**:

  ![](hello-ios-multiscreen-quickstart-images/image6.png "Set the relationship to Root")

  The **ViewController** is now our **Navigation Controller’s Root View Controller.**


8. Double-click on the **Phoneword** screen’s **Title** bar and change the **Title** to **Phoneword**:

  ![](hello-ios-multiscreen-quickstart-images/image7.png "Change the Title to Phoneword")


9. Drag a **Button** from the **Toolbox** and place it under the **Call Button**. Drag the
  handles to make the new **Button** the same width as the **Call Button**:

  ![](hello-ios-multiscreen-quickstart-images/image8.png "Make the new Button the same width as the Call Button")


10. In the **Properties Explorer**, change the **Name** of the **Button** to `CallHistoryButton`
  and change the **Title** to **Call History**:

  ![](hello-ios-multiscreen-quickstart-images/image9.png "Change the Name of the Button to 'CallHistoryButton' and the Title to 'Call History'")


11. Create the **Call History** screen. From the **Toolbox**, drag a **Table View Controller**
  onto the design surface:

  ![](hello-ios-multiscreen-quickstart-images/image10.png "Drag a Table View Controller onto the design surface")


12. Select the **Table View Controller** by clicking on the black bar at the bottom of the Scene. In the **Properties Explorer**,
  change the **Table View Controller’s** class to `CallHistoryController` and press **Enter**:

  ![](hello-ios-multiscreen-quickstart-images/image11.png "Change the Table View Controllers class to CallHistoryController")

  The iOS Designer will generate a custom backing class called `CallHistoryController` to manage this screen’s Content View Hierarchy.
  The **CallHistoryController.cs** file will appear in the **Solution Explorer**:

  ![](hello-ios-multiscreen-quickstart-images/image12.png "The CallHistoryController.cs file in the Solution Explorer")


13. Double-click on the **CallHistoryController.cs** file to open it and replace the contents with the following code:

        using System;
        using Foundation;
        using UIKit;
        using System.Collections.Generic;

        namespace Phoneword
        {
            public partial class CallHistoryController : UITableViewController
            {
                public List<String> PhoneNumbers { get; set; }

                static NSString callHistoryCellId = new NSString ("CallHistoryCell");

                public CallHistoryController (IntPtr handle) : base (handle)
                {
                    TableView.RegisterClassForCellReuse (typeof(UITableViewCell), callHistoryCellId);
                    TableView.Source = new CallHistoryDataSource (this);
                    PhoneNumbers = new List<string> ();
                }

                class CallHistoryDataSource : UITableViewSource
                {
                    CallHistoryController controller;

                    public CallHistoryDataSource (CallHistoryController controller)
                    {
                        this.controller = controller;
                    }

                    // Returns the number of rows in each section of the table
                    public override nint RowsInSection (UITableView tableView, nint section)
                    {
                        return controller.PhoneNumbers.Count;
                    }

                    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
                    {
                        var cell = tableView.DequeueReusableCell (CallHistoryController.callHistoryCellId);

                        int row = indexPath.Row;
                        cell.TextLabel.Text = controller.PhoneNumbers [row];
                        return cell;
                    }
                }
            }
        }

  Save the application and build it to ensure there are no errors. It is okay to ignore any build warnings for now.


14. Create a _Segue_ (transition) between the **Phoneword** Scene and the **Call History** Scene.
  In the **Phoneword Scene**, select the **Call History Button** and **Ctrl-drag** from the
  **Button** to the **Call History** Scene:

  ![](hello-ios-multiscreen-quickstart-images/image13.png "Ctrl-drag from the Button to the Call History Scene")

  From the **Action Segue** popover, select **Show**:

  ![](hello-ios-multiscreen-quickstart-images/image14.png "Select Show as the segue type")

  The iOS Designer will add a Segue between the two Scenes:

  ![](hello-ios-multiscreen-quickstart-images/image15.png "The Segue between the two Scenes")


15. Add a **Title** to the **Table View Controller** by selecting the black bar at the bottom of the Scene
  and changing **View Controller > Title** to **Call History** in the **Properties Explorer**:

  ![](hello-ios-multiscreen-quickstart-images/image16.png "Change the View Controller Title to Call History")


16. When the application is run, the **Call History Button** will open the **Call History** screen,
  but the Table View will be empty because there is no code to to keep track of and display the phone numbers.

  This app will store the phone numbers as a list of strings.

  Add a `using` directive for `System.Collections.Generic`  at the top of **ViewController**:

        using System.Collections.Generic;

17. Modify the `ViewController` class with the following code:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Foundation;
    using UIKit;

    namespace Phoneword_iOS
    {
      public partial class ViewController : UIViewController
      {
        string translatedNumber = "";

        public List<string> PhoneNumbers { get; set; }

        protected ViewController(IntPtr handle) : base(handle)
        {
          //initialize list of phone numbers called for Call History screen
          PhoneNumbers = new List<string>();
        }

        public override void ViewDidLoad()
        {
          base.ViewDidLoad();
          // Perform any additional setup after loading the view, typically from a nib.

          TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
            // Convert the phone number with text to a number
            // using PhoneTranslator.cs
            translatedNumber = PhoneTranslator.ToNumber(
              PhoneNumberText.Text);

            // Dismiss the keyboard if text field was tapped
            PhoneNumberText.ResignFirstResponder();

            if (translatedNumber == "")
            {
              CallButton.SetTitle("Call ", UIControlState.Normal);
              CallButton.Enabled = false;
            }
            else
            {
              CallButton.SetTitle("Call " + translatedNumber,
                UIControlState.Normal);
              CallButton.Enabled = true;
            }
          };

          CallButton.TouchUpInside += (object sender, EventArgs e) => {

            //Store the phone number that we're dialing in PhoneNumbers
            PhoneNumbers.Add(translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl("tel:" + translatedNumber);

            // otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl(url))
            {
              var alert = UIAlertController.Create("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
              alert.AddAction(UIAlertAction.Create("Ok", UIAlertActionStyle.Default, null));
              PresentViewController(alert, true, null);
            }
          };
        }

        public override void PrepareForSegue(UIStoryboardSegue segue, NSObject sender)
        {
          base.PrepareForSegue(segue, sender);

          // set the View Controller that’s powering the screen we’re
          // transitioning to

          var callHistoryContoller = segue.DestinationViewController as CallHistoryController;

          //set the Table View Controller’s list of phone numbers to the
          // list of dialed phone numbers

          if (callHistoryContoller != null)
          {
            callHistoryContoller.PhoneNumbers = PhoneNumbers;
          }
        }
      }
    }
    ```

There are a few things happening here
  * The variable `translatedNumber` was moved from the `ViewDidLoad` method to a _class-level variable_.
  * The  **CallButton** code was modified to add dialed numbers to the list of phone numbers by calling
`PhoneNumbers.Add(translatedNumber)`
  * The `PrepareForSegue` method was added

  Save and build the application to make sure there are no errors.

  Save and build the application to make sure there are no errors.


20. Press the **Start** button to launch our application inside the **iOS Simulator**:

  ![](hello-ios-multiscreen-quickstart-images/19.png "The first screen of the sample app")


Congratulations on completing your first multi-screen Xamarin.iOS application!


-----

The app can now handle navigation using both Storyboard Segues and in code. Now it’s time to dissect the tools and skills we just
learned in the [Hello, iOS Multiscreen Deep Dive](~/ios/get-started/hello-ios-multiscreen/hello-ios-multiscreen-deepdive.md).


## Related Links

- [Hello, iOS (sample)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [iOS Human Interface Guidelines](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [iOS Provisioning Portal](https://developer.apple.com/ios/manage/overview/index.action)
