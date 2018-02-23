---
title: "Connected Services in Visual Studio for Mac"
description: "Add Azure data storage, authentication, and push notifications to mobile apps from within Visual Studio for Mac"
ms.topic: article
ms.prod: xamarin
ms.assetid: ADDFB3A5-DB6A-417C-8ACE-33D107FB75C2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
---

# Connected Services Walkthrough

The Connected Services workflow brings the Azure portal workflow into Visual Studio for Mac,
so you don’t have to leave your project to add services.

This walkthrough shows how to add an Azure backend service, which brings
cloud data storage, authentication, and push notifications
to a cross-platform Xamarin.Forms Portable Class Library (PCL) application.


1.	Start by double-clicking on the **Connected Services** node in the solution, which will bring up the **Services Gallery**.
  This is a list of all the available services for this application type. Select a service
  (such as **Mobile backend with Azure App Service**) by clicking on it.

  [ ![](connected-services-images/image001-sml.png "Connected Services node in Visual Studio for Mac")](connected-services-images/image001.png)

2. The Service Details Page has a description of the service and the dependencies to be installed.
  Click the **Add** button to add the dependencies to the app:

  [ ![](connected-services-images/image002-sml.png "Mobile backend with Azure")](connected-services-images/image002.png)

3. The dependencies need to be added to both the PCL and the platform-specific projects to work.
  Select the checkboxes to add the service to every project that will reference it (either directly or indirectly):

  [ ![](connected-services-images/image003-sml.png "Check all projects that should reference the service")](connected-services-images/image003.png)

4. Choose **Accept** on the **License Acceptance** dialogs for the NuGet packages.
  There may be two dialogs to accept, one for the MobileClient and dependencies, and another for SQLiteStore, which is required for offline data sync:

  [ ![](connected-services-images/image004-sml.png "Accept License Agreements")](connected-services-images/image004.png)

  ![](connected-services-images/image005.png "License Acceptance window")

5. Once the dependencies have been added, you will be asked to log in with the account you want to use to communicate with Azure.
  If you’re already logged in with a Microsoft ID, Visual Studio for Mac will attempt to fetch your Azure subscriptions
  and any app services associated with them. If you do not have any subscriptions, you can add one by signing up for a free trial or purchasing a subscription plan in the Azure portal.

6. Select an app service from the list. This will fill the template code for the `MobileServiceClient` object with the corresponding URL of the app service on Azure:

  [ ![](connected-services-images/image006-sml.png "Select app service from list")](connected-services-images/image006.png)

  If there are no services listed, click the **New** button (see Step 9.)

7. Copy the template code for the `MobileServiceClient` into the PCL. The file location app in not important, as long as there is only one instance of it.
  The recommended approach is to create an `AzureService` class that handles all Azure interactions and uses the `MobileServiceClient`:

  ![](connected-services-images/image007.png "Copy config code into the app")

8. Follow the documentation in **Next Steps** to add data, offline sync, authentication, and push notifications to your app:

  [ ![](connected-services-images/image008-sml.png "Review the next steps instructions")](connected-services-images/image008.png)

10. If you don’t have any existing app services, you can create new services from within Visual Studio for Mac.
  Click the **New** button in the bottom left of the services list to open the **New App Service** dialog:

  [ ![](connected-services-images/image009-sml.png "Create a new app service in Visual Studio for Mac")](connected-services-images/image009.png)

A new service requires the following parameters:

-	**App service name** – unique name/id for the plan
-	**Subscription** – the subscription you’d like to use to pay for the service
-	**Resource Group** – a way or organizing all your Azure resources for a project. Option to use existing or create a new one. If this is your first Azure service, create a new one.
-	**Service Plan** – Determines the location and cost of any resources that use it. Option to use existing or create a new one. If this is your first Azure service, use the default one or create a new one in the free tier (F1).

Visit the [Azure App Service documentation](https://docs.microsoft.com/azure/app-service/) for
more information


## Related Links

- [Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/)
- [Release Notes](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/#Connected_Services)
