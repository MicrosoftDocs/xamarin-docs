---
title: "Apple Pay Capabilities in Xamarin.iOS"
description: "Adding capabilities to an application often requires additional provisioning setup. This guide explains the setup needed for Apple Pay capabilities."
ms.prod: xamarin
ms.assetid: 735CC916-16A4-471B-87F7-0535E24288D7
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/15/2017
---

# Apple Pay Capabilities in Xamarin.iOS

_Adding capabilities to an application often requires additional provisioning setup. This guide explains the setup needed for Apple Pay capabilities._

Apple Pay enables users to pay for physical goods via their iOS device. This section describes how to create all necessary components required for Apple pay in the Apple Developer Center.

When provisioning a new app through the developer center, there are three steps that need to be taken:

1. Create a Merchant ID.
2. Create an App ID with the Apply Pay capability and add the merchant to it.
3. Generate a certificate for the Merchant ID.

The steps below will guide you through creating the above items:

<a name="merchantid"></a>

## Create Merchant ID

A Merchant ID is used to let Apple Pay know that you can accept payments, and is passed to PassKit’s `PaymentRequest` method and used in the Apple Pay entitlement:

1. Browse to the [Apple Developer Center](https://developer.apple.com/account/) and go to the Certificates, Identifier, and Profiles section:

    ![Developer Center Merchant ID selection](apple-pay-capabilities-images/image57.png)

2. Under **Identifiers**, select **Merchant IDs**, and then select the **+** to create a new merchant ID:  

3. Fill out the form, illustrated below, with a new description and identifier. The description makes the ID identifiable to you and can be changed later. The identifier must be unique to you, and it must start with the string `merchant`. Apple recommends that the identifier be in the following format: `merchant.com.[Your-App-Name]`:

    ![New Merchant ID details](apple-pay-capabilities-images/image58.png)

4. Confirm the details, and **Register** your ID: 

    ![Merchant ID confirmation](apple-pay-capabilities-images/image59.png)

<a name="appid"></a>

## Create an App ID with the Apple Pay capability that includes the Merchant ID

1. In the [Developer Center](https://developer.apple.com/account/) click on **App IDs** under **Identifiers**:

    ![Select App ID in Developer Center](apple-pay-capabilities-images/image6.png)

2. Select the **+** button to add a new App ID:

    ![Add new App ID button](apple-pay-capabilities-images/image27.png)

3. Enter a Name for the App ID and give it an Explicit App ID:    

    ![App ID details screen](apple-pay-capabilities-images/image35.png)

4. Under App Services, select Apple Pay:    

    ![App Services Apple Pay](apple-pay-capabilities-images/image36.png)

5. Select **Continue** and then **Register**. Note that on the confirmation screen Apple Pay will display with Configurable selected, with a yellow symbol:

    ![Confirmation Screen for Apple Pay](apple-pay-capabilities-images/image37.png)

6. Return to the list of App IDs and select the one you have just created:  

    ![Edit App ID](apple-pay-capabilities-images/image38.png)

7. Scroll down to the bottom of this expanded section and click **Edit**.
8. Scroll down the list to Apple Pay and click the **Edit** button:  

    ![Edit Apple Pay App ID details](apple-pay-capabilities-images/image39.png)

9. Select the Merchant ID to use with this App ID, and click **Continue**:  

    ![Select Merchant ID to use for App ID](apple-pay-capabilities-images/image40.png)

10. Confirm the Merchant ID assignments, and press **Assign**:  

    ![Confirmation Screen](apple-pay-capabilities-images/image41.png)

This App ID can now be used to generate, or to re-generate, a new provisioning profile, as described in the [Working with Capabilities](~/ios/deploy-test/provisioning/capabilities/index.md) guide.

<a name="certificate"></a>

## Create a Certificate for your Merchant ID

A certificate is required by Apple to encrypt sensitive data associated with the transaction. Each Merchant ID created must have its own certificate.

To create a certificate, follow the steps below:

1. Select the Merchant ID that was created above and press **Edit**:

    ![Edit Merchant ID dialog](apple-pay-capabilities-images/image42.png)

2. On the iOS Merchant ID Settings screen, click **Create Certificate**:

    ![Create payment processing certificate](apple-pay-capabilities-images/image43.png)

3. Answer the following question:

    ![address if payments will be processed exclusively in China](apple-pay-capabilities-images/image44.png)

4. At this point you will be prompted to create a _certificate signing request_:

    ![Creating a certificate signing request](apple-pay-capabilities-images/image45.png)

    > [!IMPORTANT]
    > If you are using a payment provider for Apple Pay, such a JudoPay or Stripe, they can provide you with a properly formatted CSR that you can use at this point. Information on requesting this is found on the [Stripe](https://stripe.com/docs/apple-pay/apps#csr) site. To create your own CSR, follow the steps 5-8 below. Once you have a CSR go to step 9.

5. Open the Keychain Access application, and browse to **Keychain Access > Certificate Assistant > Request a Certificate from a Certificate Authority:**

     ![Create a CSR using keychain on a Mac](apple-pay-capabilities-images/image46.png)

6. Enter your email address, enter a name for the private key, leave CA Email Address empty, select the **Save to Disk** option, and select **Let me specify key pair information**:

     ![Certificate information dialog](apple-pay-capabilities-images/image47.png)

7. Save the CSR to a convenient location:

     ![Saving CSR to local machine](apple-pay-capabilities-images/image48.png)

8. In the Key Pair information screen, set **Key Size** to **256 bits** and **Algorithm** to **ECC** and click **Continue**:

     ![Enter key pair information dialog](apple-pay-capabilities-images/image49.png)

9. On the Developer Center, click **Continue** to upload the CSR:

     ![Prepare to upload CSR to developer center](apple-pay-capabilities-images/image50.png)

10. Click **Choose File…** to select the CSR and press **Continue** to upload it to the developer portal:

     ![Upload CSR to developer center](apple-pay-capabilities-images/image51.png)

11. Once the certificate has been generated, download it and double click on it to install it to your keychain.

For more information on using Apple Pay, refer to the following guide:

* [Introduction to Apple Pay](~/ios/platform/apple-pay.md)

## Next Steps

The list below describes additional steps that may need to be taken:

* Use the framework namespace in your app.
* Add the required entitlements to your App. Information on the entitlements required and how to add them is detailed in the [Working with Entitlements](~/ios/deploy-test/provisioning/entitlements.md) guide.
* In the App's **iOS Bundle Signing**, ensure that the **Custom Entitlements** is set to **Entitlements.plist**. This is _not_ the default setting for Debug and iOS Simulator builds.

If you encounter issues with app services, refer to the [Troubleshooting](~/ios/deploy-test/provisioning/capabilities/index.md) section of the main guide.
