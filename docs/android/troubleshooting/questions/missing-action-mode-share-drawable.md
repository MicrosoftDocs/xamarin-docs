---
title: "Android.Support.v7.AppCompat - No resource found that matches the given name: attr 'android:actionModeShareDrawable'"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
---

# Android.Support.v7.AppCompat - No resource found that matches the given name: attr 'android:actionModeShareDrawable'

1. Make sure you download the latest extras as well as the Android 5.0 (API 21) SDK via the Android SDK Manager.

2. Ensure that you are compiling your application with compileSdkVersion set to 21. You can optionally set the targetSdkVersion to 21 as well.

3. If you require a previous version such as API 19, please download the respective version found on the Nuget page:

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*Note*: If you manually install this via Package Manager Console, make sure you also install the same version of Xamarin.Android.Support.v4

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Stack Overflow Reference: [http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## See Also

- [Which Android SDK packages should I install?](~/android/troubleshooting/questions/install-android-sdk-packages.md)

