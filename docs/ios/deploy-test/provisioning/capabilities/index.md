---
title: "Working with Capabilities"
description: "Adding capabilities to an application often requires additional provisioning setup. This guide explains the setup needed for all capabilities."
ms.prod: xamarin
ms.assetid: 98A4676F-992B-4593-8D38-6EEB2EB0801C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
---

# Working with Capabilities

_Adding capabilities to an application often requires additional provisioning setup. This guide explains the setup needed for all capabilities._

Apple provides developers with _capabilities_, often known as _app services_, as a means of extending functionality and widening the scope of what iOS apps can do. The capabilities allow developers to add a deeper integration of platform features to their application, such as: the ability to have monetary transactions initiated from the app, additional device services such as Siri, and more.
These capabilities can be used with Xamarin.iOS projects. The full list of services is described below:

* App Groups
* Associated Domains
* Data Protection
* Game Center
* HealthKit
* HomeKit
* Wireless Accessory Configuration
* iCloud
* In-App Purchase
* Inter-App Audio
* Apple Pay
* Wallet
* Push Notification
* Personal VPN
* Siri
* Maps
* Background Modes
* Keychain Sharing
* Network Extensions
* Hotspot Configuration
* Multipath
* NFC Tag Reading


Capabilities can be enabled either through Visual Studio for Mac or manually in the Apple Developer Portal. Certain capabilities such as Wallet, Apple Pay, and iCloud require additional configuration of the App IDs.

This guide explains how to enable each of these App Services in your application in both Visual Studio for Mac and manually through the developer center, including any additional setup that may be required. 

## Adding App Services

To use capabilities, the app must have a valid provisioning profile that contains an App ID with the correct service enabled. Creating this provisioning profile can either be done automatically in Visual Studio for Mac or manually in the Apple Developer Center.

This section explains how to use Visual Studio for Mac's automatic provisioning or the Developer Center to enable most capabilities. There are some capabilities such as Wallet, iCloud, Apple Pay, and App Groups that require additional setup. These are explained in detail in the adjoining guides.

# [Visual Studio for Mac](#tab/vsmac)

> [!IMPORTANT]
> Not all capabilities can be added and managed in Visual Studio for Mac. The following list contains the supported capabilities:
>
>* HealthKit 
>* HomeKit 
>* Personal VPN 
>* Wireless Accessory Configuration 
>* Inter-App Audio 
>* SiriKit 
>* Hotspot 
>* Network Extensions 
>* NFC Tag Reading
>* Multipath 
>
>Push Notifications, Game Center, In-App Purchase, Maps, Keychain Sharing, Associated Domains, and Data Protection capabilities are not currently supported. To add these capabilities, use manual provisioning and follow the steps in the [Developer Center](#devcenter) section.


Capabilities are added to the **Entitlements.plist** in Visual Studio for Mac. To add capabilities, follow the steps below:

1. Open the **Info.plist** file of your iOS application and ensure **Automatically manage signing** is selected. Follow the steps in the [Automatic Provisioning](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) guide if you need help:

    ![Automatically manage signing option](images/manage-signing.png)

2. Open the **Entitlements.plist** file and select the capability that you wish to add:

    ![Add capabilities to entitlements.plist file](images/image17.png)

    Selecting a capability does two things:
    * Adds that feature to your App ID
    * Adds the entitlement key/value pair to your Entitlements.plist file.

    Visual Studio for Mac will advise you when these tasks have been carried out by displaying the following success message:

    ![Add capabilities to entitlements.plist file](images/image18.png)

# [Visual Studio](#tab/vswin)

As there is currently no support for automatic provisioning in Visual Studio 2017, you must use the [Developer Center](#devcenter) to create a an app ID with the correct application services.

-----

<!--
<a name="xcode" />

## Xcode

Xamarin developers can also use Xcode to quickly create a provisioning profile with a suitable App ID. This process, described below, can be used for any app service in the list:

1.	Open Xcode and create a ‘dummy’ project. Give the dummy project the same name as your Xamarin.iOS project. The bundle identifier should be identical to the bundle identifier of your Xamarin.iOS project:

    ![Xcode Create Project](images/image1.png)

2.	Ensure **Automatically manage signing** is selected:

    ![Automatically manage signing selection](images/image2.png)

3.	Once the app has been created, go to the tab named **Capabilities**:

    ![Xcode Capabilities tab](images/image3.png)

4.	Browse to the capability that you wish to add, and move the switch to the **ON** position.
5.	This will create a provisioning profile with an App ID that contains the capability and adds the entitlement to the profile.
6.	In Visual Studio for Mac / Visual Studio, browse to **Project Options > Bundle Signing** and set the provisioning profile to the one that was just created in Xcode:

    ![Visual Studio for Mac Project Options](images/image4.png)
-->

<a name="devcenter" />

## Developer Center

Using the developer center is a two step process that requires creating an App ID and then using that App ID to create a provisioning profile. These steps are detailed below.

### Creating an App ID with an app service

1.	Browse to the [Apple Developer Center](https://developer.apple.com/account) on a Mac (the build host mac if using a windows machine) and log in.
2.	Select **Certificates, Identifiers, and Profiles**:

    ![Apple Developer Center](images/image5.png)

3.	Under **Identifiers**, select **App IDs**:

    ![App ID selection in Developer Center](images/image6.png)

4.	Press the **+** button in the top right corner to create a new App ID.
5.	Enter an App ID description, select Explicit App ID, and enter a bundle ID in the format `com.domain.appname`. This bundle ID should match the bundle ID in your project:

    ![Adding App ID details](images/image7.png)

6.	Under **App Services** select the service or services that are required in your app:

    ![App Services selection page](images/image8.png)

7.	Press **Continue**.
8.	Confirm your App ID. Each service will be in one of the following states: **Enabled**, **Disabled**, or **Configurable**, as illustrated below. If it’s **Enabled,** it is ready to be used in a provisioning profile. If it is **Configurable**, additional setup is required for this capability. These additional steps are described in more detail in later sections.

    ![App ID confirmation](images/image9.png)

9.	Click **Register** and then **Done**. The newly created App ID should display in the iOS App IDs list.


<a name="provisioningprofile" />

### Creating a Provisioning Profile

Now create a provisioning profile that contains this App ID. Follow the steps below:

1.	In the Apple Developer Center, browse to **Provisioning Profiles > All**:

    ![Provisioning Profile section](images/image10.png)

2.	Press the **+** button in the top right corner to create a new provisioning profile.
3.	Select the type of provisioning profile that you need, and click **Continue**:

    ![Provisioning Profile selection](images/image11.png)

4.	From the dropdown list, select the App ID that was created in the steps above and press **Continue**:

    ![App ID selection](images/image12.png)

5.	Select the certificates used to sign the app and press **Continue**:

    ![Certificate selection](images/image13.png)

6.	Select the devices to be included in this profile and press **Continue**:

    ![Select devices for Provisioning Profile](images/image14.png)

7.	Give the profile a name so that it can be identified and press **Continue** to generate the profile:

    ![Name provisioning Profile](images/image15.png)

8.	Press the **Download** button to download it, and double-click on the file in Finder to install the provisioning profile.

9. If you are using Visual Studio for Mac ensure that the **Automatically manage signing** option is de-selected in the **Info.plist** file

10.	In Visual Studio for Mac / Visual Studio, browse to **Project Options > Bundle Signing** and set the provisioning profile to the one that was just created:

    ![Visual Studio for Mac Project Options](images/image16.png)

> [!IMPORTANT]
> You may also need to set entitlement keys in the Entitlement.plist file and privacy keys in the Info.plist file. More information on these entitlements is provided in the [Working with Entitlements](~/ios/deploy-test/provisioning/entitlements.md) guide.

<a name="nextsteps" />

## Next Steps

Once a Capability has been enabled on the server side, there is still work that needs to be done to allow your app to use the functionality. The list below describes additional steps that may need to be taken:

*	Use the framework namespace in your app.
*	Add the required entitlements to your App. Information on the entitlements required and how to add them is detailed in the [Introduction to Entitlements](~/ios/deploy-test/provisioning/entitlements.md) guide.

<a name="troubleshooting" />

## Troubleshooting Capabilities

The list below details some of the most common issues that can create roadblocks when developing an app with an app service enabled.

-	Ensure that the correct ID has been properly created and registered in the **Certificates, IDs & Profiles** section of Apple's Developer Portal.
-	Ensure that the Service have been added to the App's (or Extension's) ID and that the service is configured to use the App Group/Merchant ID/Container created above in the **Certificates, IDs & Profiles** of Apple's Developer Portal.
-	Ensure that the Provisioning Profiles and App IDs have been installed and that the App's **Info.plist** (in the Xamarin Project) is using one of the App IDs configured above.
-	Ensure that the App's **Entitlements.plist** file (in the Xamarin Project) has the correct service enabled.
-	Ensure that the appropriate privacy-keys are set in info.plist
-	In the App's **iOS Bundle Signing**, ensure that the **Custom Entitlements** is set to **Entitlements.plist**. This is _not_ the default setting for Debug and iOS Simulator builds.

<a name="summary" />

## Summary

This guide explained Capabilities, or _app services_, and described how they can be enabled in Visual Studio and in the Apple Developer Center. It also detailed how to set up more complicated services such as Wallet, iCloud, Apple Pay, and App Groups. Finally, it covered the next steps for getting set up and simple troubleshooting options.