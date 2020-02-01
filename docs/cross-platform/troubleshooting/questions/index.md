---
title: "General Frequently Asked Questions"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: C7E6E54D-3957-407D-BB87-22B095148C6B
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
---

# General Frequently Asked Questions

## Portable Class Libraries

### [How can I view what libraries are supported in a PCL?](pcl-support-libraries.md)
This guide lists resources and methods for determining if your existing library is supported by the various PCL target platforms, or can be converted to a PCL profile.

### [PCL Reflection API](pcl-reflection.md)
Microsoft developed a new Reflection API for use in Portable Class Libraries. If you have some existing Reflection code that you want to move to a PCL, it might not work.

### [PCL case study: How can I resolve problems related to System.Diagnostics.Tracing for the Microsoft TPL Dataflow NuGet package?](pcl-case-study.md)
Xamarin.iOS and Xamarin.Android do not implement 100% of every PCL profile that they allow as references. For practical convenience in Visual Studio for Mac, Visual Studio, and the NuGet package manager, Xamarin projects allow the use of several profiles that only have incomplete implementations. For example, neither Xamarin.iOS nor Xamarin.Android currently includes a complete implementation of the types in the `System.Diagnostics.Tracing` PCL namespace. You can work around this by switching the app project to reference the portable-net45+win8+wp8+wpa81 version of the TPL Dataflow library.

## NuGet packages & Xamarin Components
### [How can I update NuGet?](nuget-update.md)
NuGet updates, extensions, and add-ins can be found under the **Updates** tab in the **NuGet Package Manager**. Detailed navigation to find the updates in Visual Studio for Mac & Visual Studio is in this guide.

### [How do I downgrade a NuGet package?](nuget-package-downgrade.md)
Visual Studio for Mac & Visual Studio both have features for selecting older versions of packages and installing them automatically; similar to how updating packages works.

### [Missing packages error after updating NuGet packages](nuget-packages-missing.md)
This issue has mainly been reported on Xamarin.Forms sample app solutions, but the potential for this issue can happen on any project that uses NuGet packages.

### [Unifying Google Play Services Components and NuGet](gps-components-nuget.md)
There used to be several Google Play Services Components and NuGet packages, but To make things easier for developers, we've now unified our Components and NuGet packages into two. In almost every case, Google Play Services should be used. The only reason to use the (Froyo) package is if you are actively targeting Froyo.

### [Where are the components stored on my machine?](component-storage.md)
Whenever you install a Xamarin component into an App project, it gets placed in the two locations listed in this guide.

## Troubleshooting
### [Where can I find my version information and logs?](version-logs.md)
This guide details where to find most diagnostic information that can be used to troubleshoot Xamarin issues.

### [When and how should I file a bug report?](howto-file-bug.md)
This guide provides tips for filing high-quality bug reports, so that our engineers are able to determine the cause (and any potential fixes) for an issue more efficiently.

### [Why isn't Jenkins supported by Xamarin?](xamarin-jenkins.md)
Jenkins is an open-source CI suite; because of this many issues that are directly caused by the Jenkins *itself* will need to be filed as issues against where you got the code; such as the [main Jenkins repo](https://github.com/jenkinsci/jenkins), or the repo for [Jenkins.app](https://github.com/stisti/jenkins-app).

### [What project settings are required for the debugger?](debugger-settings.md)
In order for the debugger to work as expected (hit breakpoints, display debug logs, etc.), developer instrumentation and debug information display must both be enabled. This guide details how to find and activate these settings.
