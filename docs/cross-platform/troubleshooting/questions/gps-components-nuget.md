---
title: "Unifying Google Play Services Components and NuGet"
description: Learn how the Google Play Services Components and NuGet packages were unified into two Google Play Services.
ms.topic: troubleshooting
ms.service: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
no-loc: [Objective-C]
---

# Unifying Google Play Services Components and NuGet

## History

There used to be several Google Play Services Components and NuGet packages:

- Google Play Services (Froyo)
- Google Play Services (Gingerbread)
- Google Play Services (ICS)
- Google Play Services (JellyBean)
- Google Play Services (KitKat)

Google actually only ships two .jar files for Google Play Services:

- `google-play-services-froyo.jar`
- `google-play-services.jar`

The discrepancy existed because our tooling didn't properly tell `aapt.exe` what the maximum resource API Level was to be used for a given app. This meant, we received compile errors if we tried using the Google Play Services (KitKat) binding on a lower API level like Gingerbread.

## Unifying Google Play Services

In more recent versions of Xamarin.Android, we now tell `aapt.exe` what maximum resource version to use, so this problem goes away for us.

This means, there's no real reason to have separate packages for Gingerbread/ICS/JellyBean/KitKat (however we do still need a separate binding for Froyo since it's a different .jar file altogether).

To make things easier for developers, we've now unified our Components and NuGet packages into two:

- Google Play Services (Froyo) (Binds `google-play-services-froyo.jar`)
- Google Play Services (Binds `google-play-services.jar`)

### Which one should be used?

In almost every case, Google Play Services should be used. The only reason to use the (Froyo) package is if you are actively targeting Froyo. The only reason this separate .jar file exists from Google is since Froyo is on such a small percentage of devices, they themselves have decided to stop supporting it, so this .jar file is a frozen, unsupported snapshot of Google Play Services.

### Note about Gingerbread

Gingerbread does not have Fragment support by default, and because of this, some of the classes in the binding will not be usable in an app at runtime on a Gingerbread device. Classes like `MapFragment` will not work on Gingerbread, and their Support variant should be used instead `SupportMapFragment`. It's up to the developer to know which to use. This incompatibility is noted by Google in their Google Play Services documentation.

### What happens to the old Components/NuGet's?

Since they are no longer needed, we have Disabled/Delisted the following Components/NuGets:

- Google Play Services (Gingerbread)
- Google Play Services (JellyBean)
- Google Play Services (KitKat)

The existing _Google Play Services (ICS)_ Component/NuGet has been renamed to _Google Play Services_ and will be kept up to date going forward. All projects referencing one of the Disabled/Delisted packages should be updated to use this one.

The disabled Components still exist and should be restorable for projects they are still referenced in, to avoid breaking them. Similarly the delisted NuGet packages still exist and can be restored. They will not be updated going forward.
