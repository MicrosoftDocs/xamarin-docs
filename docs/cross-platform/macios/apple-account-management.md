---
title: "Apple Account Management"
ms.topic: article
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/05/2017
---

# Apple Account Management

The Apple account management interface provides a way to view all development teams associated with an Apple ID. It also allows you to view more details about each team by displaying a list of _Signing Identities_ and _Provisioning Profiles_ that are installed on your machine.

Authentication of your Apple ID is performed on the command line with [fastlane](https://fastlane.tools/). fastlane must be installed on your machine for you to be successfully authenticated. More information on fastlane and how to install it is detailed in the [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) guides.

The Apple Account dialog in Visual Studio for Mac allows you to do the following:

* **Create and Manage Certificates** 
* **Create and Manage Provisioning Profiles** 

Information on how to do this is described in this guide.

You can also use the iOS Bundle Signing tools to do the following:

* **Add a new signing identity to an existing profile** 
* **Provision new devices** 

For more information on using these features, refer to the [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) guide.
️
## Requirements

Apple account management is available on Visual Studio for Mac. It is not currently available on Visual Studio for Windows.

You must have an Apple Developer account to use this feature. More information on Apple developer accounts is available in the [Device Provisioning](~/ios/get-started/installation/device-provisioning/index.md) guide.

- Ensure you are connected to the internet. This is because fastlane communicates directly with the Apple Developer portal.
- Ensure you have [fastlane tools installed](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).
- Ensure you have the latest fastlane tools from [https://download.fastlane.tools](https://download.fastlane.tools).
- Before you begin, make sure to accept any user license agreements in the [developer portal](https://developer.apple.com/account/).

## Adding an Apple developer account

1. To open the account management dialog go to **Visual Studio > Preferences > Apple Developer Account**:

    ![Apple Developer Account options](apple-account-management-images/image1.png)

2. Press the **+** button to display the sign in dialog, as depicted below: 

    ![fastlane dialog.](apple-account-management-images/image2.png)

4. Enter your Apple ID and Password and click the **Sign In** button. This will save your credentials in the secure Keychain on this machine. [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) is used to handle your credentials securely and pass them to Apple's developer portal.
 
5. Select **Always Allow** on the alert dialog to allow Visual Studio to use your credentials :

    ![](apple-account-management-images/image4.png)

6. Once your account has been added successfully, you'll see your Apple ID and any teams that your Apple ID is part of.

    ![](apple-account-management-images/image5.png)

7. Select any team and press the **View Details…** button. This will display a list of all Signing Identities and Provisioning Profiles that are installed on your machine:

    ![](apple-account-management-images/image6.png)


<a name="managing"/>
    


## Managing Signing Identities and Provisioning Profiles

The team details dialog displays a list of Signing Identities, organized by type. The **Status** column advises you if the certificate is: 

* **Valid** – The signing identity (both the certificate and the private key) is installed on your machine and it has not expired.

* **Not in Keychain** – There is a valid signing identity on Apple's server. To install this on your machine, it must be exported from another machine. You cannot download the signing identity from the Apple Developer Portal as it will not contain the private key.

* **Private key is missing** – A Certificate with no private key is installed in the keychain.

* **Expired** – The Certificate is expired. You should remove this from your keychain.

  ![](apple-account-management-images/image7.png)

## Create a Signing Identities

To create a new signing identity, select the **Create new Certificate** drop-down button and select the type that you require. If you have the correct permissions a new signing identity will appear after a few seconds.

If an option in the drop-down is greyed out and unselected, as illustrated below, it means that you do not have the correct team permissions to create this type of certificate.

![](apple-account-management-images/image8.png)

## Download Provisioning Profiles

The team details dialog also displays a list of all provisioning profiles connected to your developer account. You can download all provisioning profiles to your local machine by pressing the **Download all Profiles** button

![](apple-account-management-images/image9.png)

## iOS Bundle Signing

For information on deploying your app to a device, refer to the [device provisioning](~/ios/get-started/installation/device-provisioning/index.md) guide.

## Troubleshooting

### View Details dialog is empty

This is currently a known issue, relating to bug [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906). Make sure that you are using the latest stable version of Visual Studio for Mac

### If you are experiencing issues logging in your account, please try the following:

* Open the keychain application and under Category select *Passwords*. Search for `deliver.`, and delete all entries.

### "Error Adding Account. Please Sign in with an app-specific password"

This is because 2 factor authentication is enabled on your account. Make sure that you are using the latest stable version of Visual Studio for Mac

### Failed to create new certificate
"You have reached the limit for certificates of this type"

![](apple-account-management-images/image10.png)

The maximum number of certificates allowed have been generated. To fix this, browse to the [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution) and revoke one of the Production Certificates.

## Known Issues

* Sometimes the View Details dialog can take an inordinate amount of time to fetch the signing identities and profiles.
* Often the focus may not return to Visual Studio for Mac after entering your details, causing your account not to be added. If this is the case, try the process again.
* Provisioning profiles created in Visual Studio for Mac will not take into account entitlements selected in your projects (Entitlements.plist). This functionality will be added in future versions of the IDE.
* Distribution provisioning profiles by default will target App Store. In House or Ad Hoc profiles should be created manually.
