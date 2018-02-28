---
title: "Upload to Mac App Store"
description: "This guide walks through uploading a Xamarin.Mac app for publication to the Mac App Store."
ms.topic: article
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
---

# Upload to Mac App Store

_This guide walks through uploading a Xamarin.Mac app for publication to the Mac App Store._

Applications are submitted for Mac App Store approval via [iTunes Connect](http://itunesconnect.apple.com/).

1. Choose a **macOS App** to create: 

	[ ![](uploading-images/image65.png "iTunes Connect")](uploading-images/image65.png)

2. Enter the application’s name and other details. The developer can only choose from an existing **Bundle ID** that has been created
previously: 

	[ ![](uploading-images/image66.png "Selecting the bundle ID")](uploading-images/image66.png)

3. Select the availability date and price. Regardless of the availability
date the developer selects, the app will only become available for sale after it has been approved. This value can be set far in the future if the developer wants more control over the actual availability date: 

	[ ![](uploading-images/image67.png "Setting the available date and price")](uploading-images/image67.png)

4. Enter the app’s information, including the App Store category it
belongs in: 

	[ ![](uploading-images/image68.png "Entering the app information")](uploading-images/image68.png) 

	Select the ratings that apply: 

	[ ![](uploading-images/image69.png "Setting the app ratings")](uploading-images/image69.png) 

	Description, keywords and contact URLs: 

	[ ![](uploading-images/image70.png "Editing the Description, keywords and contact URLs")](uploading-images/image70.png) 

	Contact information and advice for the App Store reviewers: 

	[ ![](uploading-images/image71.png "Editing the contact information and advice for the App Store reviewers")](uploading-images/image71.png) 

	And finally, screenshots: 

	[ ![](uploading-images/image72.png "Adding the required screenshots")](uploading-images/image72.png) 

	Screenshots should be in JPG, TIF or PNG format, 1280x800, 1440x900,
2880x1800 or 2560x1600 pixels in size. Press **Save** to
finish.

5. The app information is shown for review. Click **View Details** to change the status: 

	[ ![](uploading-images/image73.png "Viewing the app details")](uploading-images/image73.png)

6. In the details view, click Ready to Upload Binary to submit
The application package file: 

	[ ![](uploading-images/image74.png "Selecting Ready to Upload Binary")](uploading-images/image74.png)

7. Answer the cryptography question: 

	[ ![](uploading-images/image75.png "Answering the cryptography question")](uploading-images/image75.png)

8. The site will advise when it is ready to accept the application
package file: 

	[ ![](uploading-images/image76.png "The acceptance notification")](uploading-images/image76.png)

9. Start Application Loader and ensure to be logged in with the Apple ID.
Choose **Deliver Your App** to proceed: 

	[ ![](uploading-images/image77.png "The Application Loader interface")](uploading-images/image77.png)

10. Select from the list of applications in **Ready to Upload
Binary** status and click **Next**: 

	[ ![](uploading-images/image78.png "Selecting the app to load")](uploading-images/image78.png)

11. Review the application metadata and click **Choose...** to find the package file: 

	[ ![](uploading-images/image79.png "Reviewing the app metadata")](uploading-images/image79.png)

12. Find the package file that was built in Visual Studio for Mac using the App Store build configuration: 

	[ ![](uploading-images/image80.png "Selecting the file to upload")](uploading-images/image80.png)

13. Press **Send**: 

	[ ![](uploading-images/image81.png "Sending the app")](uploading-images/image81.png)

14. The package will be validated and any errors reported. Fix these errors and re-upload. When the upload is successfully completed, the app will be automatically submitted for review by the App Store team: 

	[ ![](uploading-images/image82.png "An example of upload errors")](uploading-images/image82.png)

When the application has been approved, it will be available for download or purchase from the Mac App Store.

## Related Links

- [Installation](~//mac/get-started/installation.md)
- [Hello, Mac sample](~//mac/get-started/hello-mac.md)
- [Distribute your apps on the Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Tools Guide : Code Signing Your App](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID and GateKeeper](https://developer.apple.com/resources/developer-id/)
