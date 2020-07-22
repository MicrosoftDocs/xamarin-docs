---
title: "Step 1. Register an app to use Azure Active Directory"
description: "This document describes how to register an Azure application with Azure Active Directory so that it can be securely accessed by mobile clients."
ms.prod: xamarin
ms.assetid: 0B17991A-4573-4F6C-9E86-D4B9D1A47E4D
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
---

# Step 1. Register an app to use Azure Active Directory

1. Navigate to [windowsazure.com](https://manage.windowsazure.com)
   and log in with your Microsoft Account or Organization Account
   in the Azure Portal. If you don’t have an Azure
   subscription, you can get a trial from
   [azure.com](https://www.azure.com)

2. After signing in, go to the **Active Directory** (1)
   section and choose the directory where you want
   to register the application (2)

   [![section and choose the directory where you want to register the application](register-images/01.-active-directory-in-azure-portal-sml.jpg)](register-images/01.-active-directory-in-azure-portal.jpg#lightbox)

3. Click **Add** to create new application, then
   select **Add an application my organization is developing**

   [![Add an application my organization is developing](register-images/02.-add-new-application-sml.jpg)](register-images/02.-add-new-application.jpg#lightbox)

4. On the next screen, give your app a name (eg. XAM-DEMO).
   Make sure you select **Native Client Application** as the type of application.

   ![Make sure you select Native Client Application as the type of application](register-images/03.-app-name.jpg)

5. On the final screen, provide a **Redirect URI* that is unique
   to your application as it will return to this URI when
   authentication is complete.

   ![On the final screen, provide a Redirect URI that is unique to your application as it will return to this URI when   authentication is complete](register-images/04.-app-redirect.jpg)

6. Once the app is created, navigate to the **Configure** tab.
   Write down the **Client ID** which we’ll use in our application
   later. Also, on this screen you can give your mobile application
   access to Active Directory or add another application like
   Web API or Office365, which can be used by mobile application once
   authentication is complete.

   ![Also, on this screen you can give your mobile application access to Active Directory or add another application like Web API or Office365](register-images/05.-configure.jpg)

## Related Links

- [Microsoft NativeClient sample](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
