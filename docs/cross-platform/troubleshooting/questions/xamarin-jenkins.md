---
title: "Why isn't Jenkins supported by Microsoft?"
description: "This document describes, at a high level, Xamarin's interaction with the Jenkins CI system. It also discusses a few common problems that come up when working with Jenkins."
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
---

# Why isn't Jenkins supported by Microsoft?

## Jenkins Support Explanation

Jenkins is an open-source CI suite; because of this many issues that are directly caused by the Jenkins *itself* will need to be filed as issues against where you got the code; such as the [main Jenkins repo](https://github.com/jenkinsci/jenkins), or the repo for [Jenkins.app](https://github.com/stisti/jenkins-app).

The exception to this is for issues that can be isolated to particular bugs in Xamarin's tools; if you suspect this to be the case you can check your [support options](~/cross-platform/troubleshooting/support-options.md), though again, the issue might be something outside what the Xamarin support team can *directly* help with.

## Setup Jenkins with Xamarin

While as noted above Jenkins issues aren't supported directly by our team; the [Using Jenkins with Xamarin](~/tools/ci/jenkins-walkthrough.md) guide can be used to set up a Jenkins CI server that's integrated with Xamarin. 

## Fixes for common issues

### Jenkins is unable to find the Android SDK

The error message for this issue is something like this:

> error XA5205: The Android SDK Directory could not be found. Please set via /p:AndroidSdkDirectory

Your options for setting the SDK location may vary depending on the exact Jenkins Android plugin you're using; a good place to look for how to set this is in the plugin guide. For example; the [Android Emulator Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) automatically looks for the SDK, but if it can't find it; the location can also be set via the Jenkins System Configuration page for that plugin. 


## Deprecated Errors

> [!IMPORTANT]
> This issue has been resolved in recent versions of Xamarin. However, if the issue occurs on the latest version of the software, please file a [new bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md) with your full versioning information and full build log output.



### Jenkins reports an invalid Xamarin license
The error messages for this issue are typically something like

> XA9008 error: Building from command line requires a business license

or

> Error: The Starter Edition of Xamarin.iOS does not support building outside of Xamarin Studio 

The most common cause of this scenario is the use of Jenkins by logging in with a user account not associated with your Xamarin license. The simplest way of resolving this, is to install Jenkins as an app directly via the user account. That process and some additional considerations are described here: [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
