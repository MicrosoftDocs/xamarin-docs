---
title: "Step 2. Configure Service Access for Mobile Application"
description: "This document describes how to provide a Xamarin application with access to an Azure application secured by Azure Active Directory."
ms.prod: xamarin
ms.assetid: 8A14A457-F72E-4B08-B4B6-801F7619F893
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
---

# Step 2. Configure Service Access for Mobile Application

Whenever any resource e.g. web application, web service,
  etc. needs to be secured by Azure Active Directory,
  it needs to be registered. All the secure applications
  or services can be seen under **Applications** tab.
  Here you can select the application which needs to be
  accessed from mobile application and give access to it.

1. On the **Configure** tab, locate **permissions to
   other applications** section:

   ![On the Configure tab, locate permissions to other applications section](configure-images/2.1-configure.png)

2. Click on **Add application** button. On the next
   screen pop-up you should see list of all the applications
   which are secured by Azure Active Directory. Select the
   applications that needs to be accessed from the mobile application.

   ![Select the applications that needs to be accessed from the mobile application](configure-images/2.2-add-application.png)

3. After selecting the application, once again select the
   newly-added application in **permissions to other
   applications** section and give appropriate rights.

   ![After selecting the application, once again select the newly-added application in permissions to other   applications section and give appropriate rights](configure-images/2.3-permissions.png)

4. Finally, **Save** the configuration. These services should
   now be available in mobile applications!

## Related Links

- [Microsoft NativeClient sample](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
