---
title: "Xamarin.Essentials Secure Storage"
description: "The SecureStorage class helps securely store simple key/value pairs."
ms.assetid: 78856C0D-76BB-406E-A880-D5A3987B7D64
author: redth
ms.author: jodick
ms.date: 05/04/2018
---
# Xamarin.Essentials Secure Storage

![Pre-release NuGet](~/media/shared/pre-release.png)

The **SecureStorage** class helps securely store simple key/value pairs.

## Using Secure Storage

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

To save a value for a given _key_ in secure storage:

```csharp
await SecureStorage.SetAsync("oauth_token", "secret-oauth-token-value");
```

To retrieve a value from secure storage:

```csharp
var oauthToken = await SecureStorage.GetAsync("oauth_token");
```

## Platform Implementation Specifics

# [Android](#tab/android)

The [Android KeyStore](https://developer.android.com/training/articles/keystore.html) is used to store the cipher key used to encrypt the value before it is saved into a [Shared Preferences](https://developer.android.com/training/data-storage/shared-preferences.html) with a filename of **[YOUR-APP-PACKAGE-ID].xamarinessentials**.  The key used in the shared preferences file is a _MD5 Hash_ of the key passed into the `SecureStorage` API's.

## API Level 23 and Higher

On newer API levels, an **AES** key is obtained from the Android KeyStore and used with an **AES/GCM/NoPadding** cipher to encrypt the value before it is stored in the shared preferences file.

## API Level 22 and Lower

On older API levels, the Android KeyStore only supports storing **RSA** keys, which is used with an **RSA/ECB/PKCS1Padding** cipher to encrypt an **AES** key (randomly generated at runtime) and stored in the shared preferences file under the key _SecureStorageKey_, if one has not already been generated.

All encrypted values will be removed when the app is uninstalled from the device.

# [iOS](#tab/ios)

[KeyChain](https://developer.xamarin.com/api/type/Android.Security.KeyChain/) is used to store values securely on iOS devices.  The `SecRecord` used to store the value has a `Service` value set to **[YOUR-APP-BUNDLE-ID].xamarinessentials**.

In some cases KeyChain data is synchronized with iCloud, and uninstalling the application may not remove the secure values from iCloud and other devices of the user.

# [UWP](#tab/uwp)

[DataProtectionProvider](https://docs.microsoft.com/en-us/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider) is used to encryped values securely on UWP devices.

Encryped values are stored in `ApplicationData.Current.LocalSettings`, inside a container with a name of **[YOUR-APP-ID].xamarinessentials**.

Uninstalling the application will cause the _LocalSettings_, and all encrypted values to be removed as well.

-----

## Limitations

This API is intended to store small amounts of text.  Performance may be slow if you try to use it to store large amounts of text.

## API

- [SecureStorage source code](https://github.com/xamarin/Essentials/tree/master/Essentials/SecureStorage)
- [SecureStorage API documentation](xref:Xamarin.Essentials.SecureStorage)
