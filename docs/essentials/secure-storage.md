---
title: "Xamarin.Essentials: Secure Storage"
description: "This document describes the SecureStorage class in Xamarin.Essentials, which helps securely store simple key/value pairs. It discusses how to use the class, platform implementation specifics, and limitations."
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
---

# Xamarin.Essentials: Secure Storage

![Pre-release NuGet](~/media/shared/pre-release.png)

The **SecureStorage** class helps securely store simple key/value pairs.

## Getting Started

To access the **SecureStorage** functionality, the following platform-specific setup is required:

# [Android](#tab/android)

No additional setup required.

# [iOS](#tab/ios)

When developing on the **iOS simulator**, enable the **Keychain** entitlement and add a keychain access group for the application's bundle identifier. 

Open the **Entitlements.plist** in the iOS project and find the **Keychain** entitlement and enable it. This will automatically add the application's identifier as a group.

In the project properties, under **iOS Bundle Signing** set the **Custom Entitlements** to **Entitlements.plist**.

> [!TIP]
> When deploying to an iOS device this entitlement is not required and should be removed.

# [UWP](#tab/uwp)

No additional setup required.

-----

## Using Secure Storage

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

To save a value for a given _key_ in secure storage:

```csharp
try
{
  await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

To retrieve a value from secure storage:

```csharp
try
{
  var oauthToken = await SecureStorage.GetAsync("oauth_token");
}
catch (Exception ex)
{
  // Possible that device doesn't support secure storage on device.
}
```

> [!NOTE]
> If there is no value associated with the requested key, `GetAsync` will
> return `null`.

To remove a specific key, call:

```csharp
SecureStorage.Remove("oauth_token");
```

To remove all keys, call:

```csharp
SecureStorage.RemoveAll();
```


## Platform Implementation Specifics

# [Android](#tab/android)

The [Android KeyStore](https://developer.android.com/training/articles/keystore.html) is used to store the cipher key used to encrypt the value before it is saved into a [Shared Preferences](https://developer.android.com/training/data-storage/shared-preferences.html) with a filename of **[YOUR-APP-PACKAGE-ID].xamarinessentials**.  The key used in the shared preferences file is a _MD5 Hash_ of the key passed into the `SecureStorage` APIs.

## API Level 23 and Higher

On newer API levels, an **AES** key is obtained from the Android KeyStore and used with an **AES/GCM/NoPadding** cipher to encrypt the value before it is stored in the shared preferences file.

## API Level 22 and Lower

On older API levels, the Android KeyStore only supports storing **RSA** keys, which is used with an **RSA/ECB/PKCS1Padding** cipher to encrypt an **AES** key (randomly generated at runtime) and stored in the shared preferences file under the key _SecureStorageKey_, if one has not already been generated.

**SecureStorage** uses the [Preferences](preferences.md) API and follows the same data persistence outlined in the [Preferences](preferences.md#persistence) documentation. If a device upgrades from API level 22 or lower to API level 23 and higher, this type of encryption will continue to be used unless the app is uninstalled or **RemoveAll** is called.

# [iOS](#tab/ios)

[KeyChain](https://developer.xamarin.com/api/type/Security.SecKeyChain/) is used to store values securely on iOS devices.  The `SecRecord` used to store the value has a `Service` value set to **[YOUR-APP-BUNDLE-ID].xamarinessentials**.

In some cases KeyChain data is synchronized with iCloud, and uninstalling the application may not remove the secure values from iCloud and other devices of the user.

# [UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) is used to encrypted values securely on UWP devices.

Encrypted values are stored in `ApplicationData.Current.LocalSettings`, inside a container with a name of **[YOUR-APP-ID].xamarinessentials**.

**SecureStorage** uses the [Preferences](preferences.md) API and follows the same data persistence outlined in the [Preferences](preferences.md#persistence) documentation.

-----

## Limitations

This API is intended to store small amounts of text.  Performance may be slow if you try to use it to store large amounts of text.

## API

- [SecureStorage source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/SecureStorage)
- [SecureStorage API documentation](xref:Xamarin.Essentials.SecureStorage)
