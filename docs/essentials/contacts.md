---
title: "Xamarin.Essentials: Contacts"
description: "The Contacts class in Xamarin.Essentials lets a user pick a contact and retrieve information about it."
ms.assetid: 02280c42-720a-44c3-979e-4818a20c9821
author: jamesmontemagno
ms.author: jamont
ms.date: 09/22/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Essentials: Contacts

The **Contacts** class lets a user pick a contact and retrieve information about it.

![Pre-release API](~/media/shared/preview.png)

## Get started

[!include[](~/essentials/includes/get-started.md)]

To access the **Contacts** functionality the following platform specific setup is required.

# [Android](#tab/android)

The `ReadContacts` permission is required and must be configured in the Android project. This can be added in the following ways:

Open the **AssemblyInfo.cs** file under the **Properties** folder and add:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.ReadContacts)]
```

OR Update Android Manifest:

Open the **AndroidManifest.xml** file under the **Properties** folder and add the following inside of the **manifest** node.

```xml
<uses-permission android:name="android.permission.READ_CONTACTS" /> />
```

Or right click on the Android project and open the project's properties. Under **Android Manifest** find the **Required permissions:** area and check this permission. This will automatically update the **AndroidManifest.xml** file.

# [iOS](#tab/ios)

In your `Info.plist` add the following keys:

```xml
<key>NSContactsUsageDescription</key>
<string>This app needs access to contacts to pick a contact and get info.</string>
```

Ensure that you update the `<string>` in the description that is specific for your app as it will be shown to your users.

# [UWP](#tab/uwp)

In the `Package.appxmanifest` under **Capabilities** ensure that `Contact` capability is checked.

-----

## Picking a Contact

By calling `Contacts.PickContactAsync()` the contact dialog will appear and allow the user to receive information about the user.


```csharp
try
{
    var contact = await Contacts.PickContactAsync();

    if(contact == null)
        return;

    var name = contact.Name;
    var contactType = contact.ContactType; // Unknown, Personal, Work
    var numbers = contact.Numbers; // List of phone numbers
    var emails = contact.Emails; // List of email addresses 
    
}
catch (Exception ex)
{
    // Handle exception here.
}
```


## API

- [Contacts source code](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Contacts)
- [Contacts API documentation](xref:Xamarin.Essentials.Contacts)
