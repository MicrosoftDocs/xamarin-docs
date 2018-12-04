---
title: "Xamarin.Essentials: Troubleshooting"
description: "This document describes how to troubleshoot issues encountered when developing with the Xamarin.Essentials library."
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
---

# Xamarin.Essentials: Troubleshooting

## Error: Version conflict detected for Xamarin.Android.Support.Compat

The following error may occur when updating NuGet packages (or adding a new package) with a Xamarin.Forms
project that uses Xamarin.Essentials:

```error
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 0.8.0-preview -> Xamarin.Android.Support.CustomTabs 27.0.2.1 -> Xamarin.Android.Support.Compat (= 27.0.2.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

The problem is mismatched dependencies for the two NuGets. This can be resolved by manually adding a specific
version of the dependency (in this case **Xamarin.Android.Support.Compat**) that can support both.

To do this, add the NuGet that is the source of the conflict manually, and use the **Version** list to select a specific version. Currently version 27.0.2.1 of the Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util NuGet will resolve this error.

Refer to [this blog post](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/)
for more information and a video on how to resolve the issue.

If run into any issues or find a bug please report it on the [Xamarin.Essentials GitHub repository](http://github.com/xamarin/Essentials).
