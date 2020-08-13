---
title: "Deploy checkboxes disabled in Configuration Manager"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
---

# Deploy checkboxes disabled in Configuration Manager

Since Xamarin 3.5, Xamarin.iOS projects deployed automatically whenever you press the **Start** toolbar button or pick the **Debug > Start Debugging** menu item. You will still need to set the desired Xamarin.iOS app project as the **StartUp Project** before running either of those commands.

Because of this, the **Deploy** checkboxes are intentionally disabled in the Visual Studio Configuration Manager for Xamarin.iOS projects:

![Visual Studio Configuration Manager showing the Deploy checkbox disabled for a Xamarin.iOS project in Xamarin 3.5](deploy-checkboxes-images/configuration.png)

This change eliminates an error that could appear in older versions of Xamarin (version 3.3 and earlier) when the Xamarin.iOS app project was not set to deploy:

![Error dialog: The project iPhoneApp1 needs to be deployed before it can be started. Verify the project is selected to be deployed in the Solution Configuration Manager.](deploy-checkboxes-images/error.png)
