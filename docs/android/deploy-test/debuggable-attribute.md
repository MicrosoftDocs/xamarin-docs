---
title: "Debuggable Attribute"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
---

# Debuggable Attribute



To make debugging possible, Android supports the Java Debug Wire Protocol (JDWP). This is a technology that allows tools such as ADB to communicate with a JVM. While JDWP is important during development, it should be disabled prior to the applicaiton being published.

JDWP can be the value of the `android:debuggable` attribute in an Android application. Xamarin.Android provides the following ways to set this attribute:

1.  Created a  `AndroidManifext.xml` file, and setting the  `android:debuggable` attribute there.
1.  Including the  `ApplicationAttribute` in a  `.CS` file like so :  `[assembly: Application(Debuggable=false)]` .


If both the `AndroidManifest.xml` and `ApplicationAttribute` are present, the contents of `AndroidManifest.xml` take priority over what is specified by the `ApplicationAttribute`.

If neither `AndroidManifest.xml` and `ApplicationAttribute`, then the default value of the `android:debuggable` attribute depends on whether or not debug symbols are generated. If debug symbols are present, then Xamarin.Android will set the `android:debuggable` attribute to `true`.

Note that the value of the `android:debuggable` attribute does NOT necessarily depend on the build configuration. It is possible for release builds to have the `android:debuggable` attribute set to true.


## Related Links

- [Debuggable apps in the Android market](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
