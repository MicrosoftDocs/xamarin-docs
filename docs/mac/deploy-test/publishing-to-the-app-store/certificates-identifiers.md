---
title: "Certificates and identifiers in Xamarin.Mac"
description: "This guide walks through creating the necessary Certificates and Identifiers that will be required to publish a Xamarin.Mac app."
ms.prod: xamarin
ms.assetid: 393d0066-7f6f-4ac3-a48d-4b5db65bc4cd
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
---

# Certificates and identifiers in Xamarin.Mac

_This guide walks through creating the necessary certificates and identifiers that will be required to publish a Xamarin.Mac app._

## Certificates and identifiers

Visit the [Apple Developer Member Center](http://developer.apple.com) to configure the Mac for development. The main menu is shown below:

[![The Apple Developer Member Center](certificates-identifiers-images/devcenter01.png "The Apple Developer Member Center")](certificates-identifiers-images/devcenter01-large.png#lightbox)

Click on the **Certificates, Identifiers & Profiles** link:

[![Selecting Certificates, Identifiers & Profiles](certificates-identifiers-images/devcenter02.png "Selecting Certificates, Identifiers & Profiles")](certificates-identifiers-images/devcenter02-large.png#lightbox)

Next, click on the **Certificates Link** under the **Mac Apps** section:

[![Selecting the Certificates link](certificates-identifiers-images/devcenter03.png "Selecting the Certificates link")](certificates-identifiers-images/devcenter03-large.png#lightbox)

Click on the **All** link and click the **+** button:

[![Selecting all and adding a new item](certificates-identifiers-images/certif01.png "Selecting all and adding a new item")](certificates-identifiers-images/certif01-large.png#lightbox)

From here download the **Intermediate Certificates** (Worldwide Developer Relations Certificate Authority and Developer ID Certificate Authority) if required. However, these should automatically be setup for the developer by Xcode.

The remainder of this section walks through each of the four sections to
complete a Mac Developer Account setup.

-   **Register Mac App ID** – The developer will need to follow these steps for each application they write.
-   **Register macOS Systems** – This is only required when adding computers to test with.
-   **Create Certificates** – Only required once when seting-up the certificates, and later when renewing them.
-   **Create Provisioning Profile** – The developer will need to follow these steps for each new application written, and when adding new systems.

Click the **Overview** link at the top left of the page
to return to this menu at any time.

### Register Mac App ID

The developer needs to register an App ID for each application written. Use the steps below to create an entry for a basic sample app called “MacWriter”.

1. Enter a **App ID Description** and select any **App Services** that the application will require: 

	[![Entering the description and app services](certificates-identifiers-images/devcenter04.png "Entering the description and app services")](certificates-identifiers-images/devcenter04-large.png#lightbox)
2. Enter a **Bundle ID** for the app and click the **Continue** button: 

	[![Entering a bundle ID](certificates-identifiers-images/devcenter05.png "Entering a bundle ID")](certificates-identifiers-images/devcenter05-large.png#lightbox)
3. Verify the information and click the **Submit** button: 

	[![Verifying the information](certificates-identifiers-images/devcenter06.png "Verifying the information")](certificates-identifiers-images/devcenter06-large.png#lightbox)

Some **App Services** might require further configuration (for example, iCloud). If that is the case, select the new App ID just created and click the **Edit** button:

[![Editing the new App ID](certificates-identifiers-images/devcenter07.png "Editing the new App ID")](certificates-identifiers-images/devcenter07-large.png#lightbox)

To configure the iCloud services, click the **Edit** button:

[![Configuring the iCloud services](certificates-identifiers-images/devcenter08.png "Configuring the iCloud services")](certificates-identifiers-images/devcenter08-large.png#lightbox)

From here the developer can configure the databases that they will be using:

[![Configuring the databases](certificates-identifiers-images/devcenter09.png "Configuring the databases")](certificates-identifiers-images/devcenter09-large.png#lightbox)

### Register macOS systems

To create a provisioning profile for testing, the developer will need to have their Mac
computers registered. They can register a maximum of 100 computers for testing their Mac Apps.

In the Mac Developer Center, select **All** from the **Devices** section and click the **+** button:

[![Adding a new computer](certificates-identifiers-images/devcenter10.png "Adding a new computer")](certificates-identifiers-images/devcenter10-large.png#lightbox)

Enter a **Name** and the **UUID** of the computer to add and click the **Continue** button. Review the information and the click **Register** button:

[![Entering the new computer information](certificates-identifiers-images/devcenter11.png "Entering the new computer information")](certificates-identifiers-images/devcenter11-large.png#lightbox)

### Create certificates

Use the Certificates section to create several different types of certificates that will be used to sign Mac Applications:

[![Creating a new certificate](certificates-identifiers-images/certif01.png "Creating a new certificate")](certificates-identifiers-images/certif01-large.png#lightbox)

There are three main types of certificate:

-   **Mac Development Certificate** – Optional for general app development, but required if the developer plans to use features like iCloud or push notifications. The developer will need a Development Certificate before they can create Provisioning Profiles that allow them to access those features.
-   **Mac App Store** – The developer will need a certificate for their app and another certificate for the installer.
-   **Developer ID** – Certificates for the app and installer if choosing to distribute outside the Mac App Store.

The following sections will provide examples of creating each of the above certificate types.

#### Mac development certificate

As mentioned previously, Mac App Development Certificate isn't required unless macOS features like iCloud or push notifications are being used.

Do the following to created a new Development Certificate:

1. Select the **Mac Development** radio button and click **Continue**: 

	 [![Adding a development certificate](certificates-identifiers-images/certif02.png "Adding a development certificate")](certificates-identifiers-images/certif02-large.png#lightbox)
2. The next screen explains how to use Keychain Access to create a certificate signing request file to upload: 

	[![The keychain access upload screen](certificates-identifiers-images/certif03.png "The keychain access upload screen")](certificates-identifiers-images/certif03-large.png#lightbox)
3. Choose a meaningful Common Name for the certificate, so that it’s easily recognizable later when the final certificate is created. Remember where the file is saved, so it can be found in the next step: 

	 ![Exporting a certificate](certificates-identifiers-images/image12.png "Exporting a certificate")
4. A certificate request file (extension `.certSigningRequest`) will
be saved locally on the Mac. Remember where it is saved (the default locationis the Desktop) it will need to be chosen in the next step: 

	 [![Uploading the certificate file](certificates-identifiers-images/image13.png "Uploading the certificate file")](certificates-identifiers-images/image13-large.png#lightbox)
5. Click **Download** to get the certificate and double-click to install it in the **Keychain**: 

	 [![Downloading a development certificate](certificates-identifiers-images/image15.png "Downloading a development certificate")](certificates-identifiers-images/image15-large.png#lightbox)
6. Click **Download** to get the certificate and double-click to install it in the **Keychain**. The **Developer Certificate Utility** will show the certificates like this: 

	 [![The Developer Certificate Utility](certificates-identifiers-images/image16.png "The Developer Certificate Utility")](certificates-identifiers-images/image16-large.png#lightbox)
7. It will also appear in the **Keychain** like this: 

	 ![The certificate in Keychain Access](certificates-identifiers-images/image17.png "The certificate in Keychain Access")

As previously mentioned the Developer certificate is not always required
unless the developer is implementing macOS features like iCloud and push notifications. It is also required to create a **Development Provisioning Profile**, which will be needed to test Mac App Store apps.

#### Mac App Store certificates

To release an app on the App Store, **Mac App Store** certificate that will be used to sign the application and the Mac Installer Package will need to be created.

1. Select **Mac App Store** as the certificate type and click the **Continue** button: 

	[![Creating an App Store Certificate](certificates-identifiers-images/certif04.png "Creating an App Store Certificate")](certificates-identifiers-images/certif04-large.png#lightbox)
2. Select the type of certificate that to create (one of each type to release to the App Store will be needed): 

	[![Selecting the certificate type](certificates-identifiers-images/certif05.png "Selecting the certificate type")](certificates-identifiers-images/certif05-large.png#lightbox)
3. The next page explains how to use **Keychain Access** to
generate a certificate request file. Follow the instructions: 

	 [![Generating the keychain request](certificates-identifiers-images/certif06.png "Generating the keychain request")](certificates-identifiers-images/certif06-large.png#lightbox)
4. Choose a descriptive **Common Name** – for example use
the text “App Store Application” in the name: 

	 ![Entering a descriptive name](certificates-identifiers-images/image20.png "Entering a descriptive name")
5. A certificate request file (extension `.certSigningRequest`) will
be saved locally on the Mac. Remember where it is saved (the default location is the Desktop): 

	 [![Saving the certificate](certificates-identifiers-images/image21.png "Saving the certificate")](certificates-identifiers-images/image21-large.png#lightbox)
6. Click **Download** to get your certificate and double-click to install it in the **Keychain**: 

	  [![Downloading the App Store certificate](certificates-identifiers-images/image23.png "Downloading the App Store certificate")](certificates-identifiers-images/image23-large.png#lightbox)
7. Click **Continue** and follow the exact same steps to download another certificate, this time it will be for the *installer*: 

	 [![Selecting the installer](certificates-identifiers-images/image24.png "Selecting the installer")](certificates-identifiers-images/image24-large.png#lightbox)
8. Choose a descriptive **Common Name** – for example use
the text “AppStore Installer” in the name: 

	 ![Setting the name of the certificate](certificates-identifiers-images/image25.png "Setting the name of the certificate")
9. A certificate request file (extension `.certSigningRequest`) will
be saved locally on the Mac. Remember where it is saved (the default location is the Desktop): 

	 [![Uploading the certificate](certificates-identifiers-images/image26.png "Uploading the certificate")](certificates-identifiers-images/image26-large.png#lightbox) 

	 [![Downloading the distribution certificate](certificates-identifiers-images/image28.png "Downloading the distribution certificate")](certificates-identifiers-images/image28-large.png#lightbox)
10. Click **Download** to get the certificate and double-click to install it in the **Keychain**. The Developer Certificate Utility will show the certificates like this: 

	 [![The Developer Certificate Utility](certificates-identifiers-images/image29.png "The Developer Certificate Utility")](certificates-identifiers-images/image29-large.png#lightbox)
11. The two new certificates will now be visible in the **Keychain**: 

	 [![The certificate in Keychain Access](certificates-identifiers-images/image30.png "The certificate in Keychain Access")](certificates-identifiers-images/image30-large.png#lightbox)

#### Developer ID certificates

To self-release a Xamarin.Mac application (not release via the Apple App Store), the developer will need a Developer ID Certificate to sign the app for release and installation.

Do the following:

1. From the **Certificates** section, start by click the **+** button, then select the **Developer ID** radio button: 

	[![Adding a developer ID](certificates-identifiers-images/certif07.png "Adding a developer ID")](certificates-identifiers-images/certif07-large.png#lightbox)
2. Click the **Continue** button and select the type of Developer ID to create: 

	[![Selecting the developer ID type](certificates-identifiers-images/certif08.png "Selecting the developer ID type")](certificates-identifiers-images/certif08-large.png#lightbox)
3. Two will be required, one to sign the application itself and one to sign the application's installer. Be careful when naming the certificate requests for these keys: use descriptive names that include the text `Application` and `Installer` so they can be distinguished later.
4. The next screen will gives detailed directions on how to create the certificate, click the **Continue** button: 

	[![How to create the certificate](certificates-identifiers-images/certif09.png "How to create the certificate")](certificates-identifiers-images/certif09-large.png#lightbox)
5. Choose a descriptive **Common Name** – for example use
the text “Developer ID Application” in the name: 

	 ![Entering a name for the certificate](certificates-identifiers-images/image33.png "Entering a name for the certificate")
6. A certificate request file (extension `.certSigningRequest`) will
be saved locally on your Mac. Remember where it is saved (the default location is the Desktop): 

	 [![Uploading the certificate](certificates-identifiers-images/certif10.png "Uploading the certificate")](certificates-identifiers-images/certif10-large.png#lightbox) 

	 [![Downloading the developer ID](certificates-identifiers-images/certif11.png "Downloading the developer ID")](certificates-identifiers-images/certif11-large.png#lightbox)
7. Click **Download** to get the certificate and
double-click to install it in the **Keychain**.
8. Click **Continue** and follow the exact same steps to download another certificate, this time it will be for the *installer*.
9. Choose a descriptive **Common Name** – for example use
the text “Developer ID Installer” in the name: 

	 ![Entering a common name](certificates-identifiers-images/image38.png "Entering a common name")
10. A certificate request file (extension `.certSigningRequest`) will
be saved locally on the Mac. Remember where it is saved (the default location is the Desktop): 

	 [![Uploading a certificate](certificates-identifiers-images/certif10.png "Uploading a certificate")](certificates-identifiers-images/certif10-large.png#lightbox)
11. The certificate is then available for download – click the **Download** button before clicking **Done**: 

	 [![Downloading a certificate](certificates-identifiers-images/certif11.png "Downloading a certificate")](certificates-identifiers-images/certif11-large.png#lightbox)
12. Click **Download** to get the certificate and double-click to install it in the **Keychain**. The **Developer Certificate Utility** will show the certificates like this: 

	 [![The Developer Certificate Utility](certificates-identifiers-images/certif12.png "The Developer Certificate Utility")](certificates-identifiers-images/certif12-large.png#lightbox)
13. The following items visible in the **Keychain**: 

	 [![The certificate in Keychain access](certificates-identifiers-images/image43.png "The certificate in Keychain access")](certificates-identifiers-images/image43-large.png#lightbox)


## Related Links

- [Installation](/visualstudio/mac/installation/)
- [Hello, Mac sample](~/mac/get-started/hello-mac.md)
- [Distribute your apps on the Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Developer ID and GateKeeper](https://developer.apple.com/resources/developer-id/)
