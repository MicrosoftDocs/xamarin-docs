---
title: "Upload to Mac App Store"
description: "This document describes how to use iTunes Connect to upload a Xamarin.Mac app to the Mac App Store. It discusses the information required by iTunes Connect to complete the process."
ms.prod: xamarin
ms.assetid: 30cd0e47-1b2e-47ef-93f6-4bed20b15c03
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
---

# Upload to Mac App Store

_This guide walks through uploading a Xamarin.Mac app for publication to the Mac App Store._

Applications are submitted for Mac App Store approval via [iTunes Connect](https://itunesconnect.apple.com/). You will also need the [**Transporter**](https://apps.apple.com/us/app/transporter/id1450874784?mt=12) tool from the App Store.

1. Choose a **macOS App** to create:

    [![iTunes Connect](uploading-images/image65.png)](uploading-images/image65.png#lightbox)

2. Enter the application’s name and other details. The developer can only choose from an existing **Bundle ID** that has been created
previously:

    [![Selecting the bundle ID](uploading-images/image66.png)](uploading-images/image66.png#lightbox)

3. Select the availability date and price. Regardless of the availability
date the developer selects, the app will only become available for sale after it has been approved. This value can be set far in the future if the developer wants more control over the actual availability date:

    [![Setting the available date and price](uploading-images/image67.png)](uploading-images/image67.png#lightbox)

4. Enter the app’s information, including the App Store category it
belongs in:

    [![Entering the app information](uploading-images/image68.png)](uploading-images/image68.png#lightbox)

    Select the ratings that apply:

    [![Setting the app ratings](uploading-images/image69.png)](uploading-images/image69.png#lightbox)

    Description, keywords and contact URLs:

    [![Editing the Description, keywords and contact URLs](uploading-images/image70.png)](uploading-images/image70.png#lightbox)

    Contact information and advice for the App Store reviewers:

    [![Editing the contact information and advice for the App Store reviewers](uploading-images/image71.png)](uploading-images/image71.png#lightbox)

    And finally, screenshots:

    [![Adding the required screenshots](uploading-images/image72.png)](uploading-images/image72.png#lightbox)

    Screenshots should be in JPG, TIF or PNG format, 1280x800, 1440x900,
2880x1800 or 2560x1600 pixels in size. Press **Save** to
finish.

5. The app information is shown for review. Click **View Details** to change the status:

    [![Viewing the app details](uploading-images/image73.png)](uploading-images/image73.png#lightbox)

6. In the details view, click Ready to Upload Binary to submit
The application package file:

    [![Selecting Ready to Upload Binary](uploading-images/image74.png)](uploading-images/image74.png#lightbox)

7. Answer the cryptography question:

    [![Answering the cryptography question](uploading-images/image75.png)](uploading-images/image75.png#lightbox)

8. The site will advise when it is ready to accept the application
package file:

    [![The acceptance notification](uploading-images/image76.png)](uploading-images/image76.png#lightbox)

9. Start **Transporter** and login with your Apple ID, then choose **ADD APP**:

    [![The Application Loader interface](uploading-images/transporter01-sml.png)](uploading-images/transporter01.png#lightbox)

    Follow the instructions to upload your app package to iTunes Connect.

    > [!NOTE]
    > [**Transporter**](https://apps.apple.com/us/app/transporter/id1450874784?mt=12) replaces
    > the **Application Loader** tool that was used with Xcode 10 and earlier.
    > Application Loader is no longer available in Xcode 11 or newer.

When the application has been approved, it will be available for download or purchase from the Mac App Store.

## Related links

- [Installation](~//mac/get-started/installation.md)
- [Hello, Mac sample](~/mac/get-started/hello-mac.md)
- [Distribute your apps on the Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Tools Guide : Code Signing Your App](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Developer ID and GateKeeper](https://developer.apple.com/developer-id/)
