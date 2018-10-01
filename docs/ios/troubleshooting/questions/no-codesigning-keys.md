---
title: "Why does my iOS build fail with: no valid iPhone code signing keys found in keychain?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/03/2018
---

# Why does my iOS build fail with: no valid iPhone code signing keys found in keychain?

## Cause of the error
This error message occurs when the project in question is looking for valid code-signing credentials but are unable to find them. Code signing is required for testing and deployments on physical iOS devices; as well as Ad-hoc & App store builds. 


### Provisioning Devices
If you haven't provisioned an iOS device before, the following guide will take you through the full step-by-step process: [Device Provisioning Guide](~/ios/get-started/installation/device-provisioning/index.md)


## Bug when using iOS Simulator

> [!NOTE]
> This issue has been resolved in recent versions of Xamarin for Visual Studio. However, if the issue occurs on the latest version of the software, please file a [new bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md) with your full versioning information and full build log output.


There was a bug in Xamarin.Visual Studio 3.11 which caused the iOS project in a Xamarin.Forms template to add the codesign Entitlements.plist to Simulator builds; effectively blocking testing using the Simulator.

### How to fix
You can workaround the issue by removing the `<CodesignEntitlements>` flag from the debug builds in the .csproj file. You can do this as follows:

*Warning: Errors in .csproj files can break your project, so it's a good idea to backup your files before attempting this.*

1. Right click the iOS project in the solution pane and select **Unload Project**
2. Right click the project again and select **Edit [ProjectName].csproj**
3. Locate the Debug PropertyGroups, they should start with flags that look like this:
   - Debug: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - Release: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. In each of the builds that use the simulator, delete or comment out the following property:
`<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. Reload the project and you should be able to deploy to the simulator.

### Next Steps
For further assistance, to contact us, or if this issue remains even after utilizing the above information, please see [What support options are available for Xamarin?](~/cross-platform/troubleshooting/support-options.md) for information on contact options, suggestions, as well as how to file a new bug if needed. 
