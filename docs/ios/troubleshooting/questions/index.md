---
title: "Xamarin.iOS Frequently Asked Questions"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 65E04188-185D-493D-BA3C-A89711CB6CAF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
---

# iOS Frequently Asked Questions

## General Questions

### [Can I use a Mac VM with Xamarin?](mac-vm.md)
Yes, but only on Mac hardware.

### [How can I downgrade Xcode?](./previous-xcode.md)
This guide provides links to access previous versions of Xcode as well as the latest version.

### [Where can I set my iOS SDK Locations?](ios-sdk.md)
For most users these are set to the proper locations automatically. This guide lists the default SDK locations and how to change them if needed.

### [How can I reenable developer options after updating iOS?](update-developer-options.md)
An iOS bug may cause the developer options to disappear after updating iOS versions, this has been observed when switching to iOS 8.x. This guide describes how the options can be reenabled.

### [User Location not working in iOS 8](ios8-user-location.md)
This guide tells you how to edit info.plist to enable User Location in iOS 8.

### [Where can I find the .dSYM file to symbolicate iOS crash logs?](symbolicate-ios-crash.md)
This guide describes basic steps for symbolicating iOS crash logs to help with diagnosing crashes. It also links to additional resources for more advanced symbolication techniques & info on interpreting iOS crash logs.

### [How do I set Mono Runtime environment variables for iOS projects in Xamarin Studio?](xs-mono-runtime.md)
If you need to set any runtime environment variables for Mono, they can be set in the **Project Options > Run > General** page.

## Publishing Questions

### [Error when submitting to App Store: “Invalid Bundle - Options not allowed to be embedded in bitcode are detected in the submission”](invalid-bundle-bitcode.md)

Submitting apps that _require_ bitcode, such as watchOS and tvOS apps,
must be done with Xcode 9.

### [Can I change the output path of the IPA file?](ipa-output-path.md)
As of Xamarin Cycle 7, you can use customized MSBuild targets to achieve this.

### [How can I copy IPA output files to the TFS drop folder?](ipa-tfs.md)
Yes, this guide describes how.

### [Can I add files to or remove files from an IPA file after building it in Visual Studio?](modify-ipa.md)
Yes, it is possible but it will usually require that you re-sign the `.app` bundle after making the change. Note that modifying the `.ipa` file is not necessary in normal use. This article is provided purely for informational purposes.

### [Is it possible to create a .xcarchive archive from Visual Studio?](create-xcarchive.md)
As of Xamarin 4, it is now possible to create a `.xcarchive` from Windows by setting the `ArchiveOnBuild` property to `true`.

### [Why does my app submission fail with: "Disallowed paths ( "iTunesMetadata.plist" ) found at ..." ?](itunesmetadata-disallowed-paths.md)
This error is the result of a change in Apple's App Store verification process. This specific error is _not_ related to the particular version of Xamarin you have installed, so downgrading will _not_ help. This guide links to more information on how to fix the issue.

## Diagnosing Specific Error Messages

### [iOS Designer Error with RegisterServicePort](error-registerserviceport.md)
Errors with `RegisterServicePort` and similar error messages like above are commonly an issue with spyware/malware on the computer. This guide details confirming the diagnosis and info on removing the spyware/malware.

### [Why does my iOS build fail with: no valid iPhone code signing keys found in keychain?](no-codesigning-keys.md)
This error message occurs when the project in question is looking for valid code-signing credentials but are unable to find them. Code signing is required for testing and deployments on physical iOS devices; as well as Ad-hoc & App store builds.

### [Why does my iOS 9 app fail with: System.Exception: Failed to marshal the Objective-C object?](exception-marshal-obj-c.md)
API changes in iOS 9 require that a callback constructor be used when calling unmanaged code, as the underlying API now expects it.

### [Runtime error: The assembly mscorlib.dll was not found or could not be loaded](error-mscorlib-not-found.md)
This issue occurs when the *hidden* `.monotouch-32` and `.monotouch-64` folders are missing from the `.xcarchive` for signing / IPA creation, triggering the runtime error.

### [Compile error: Can not encode offset X in resulting scattered relocation](error-encode-offset-scattered-relocation.md)
This issue occurs when building for 32-bit architectures, such as ARMv7, when the final binary is too large for the native toolchain.

## Deprecated

> [!IMPORTANT]
> The articles below apply to issues that have been resolved in recent versions of Xamarin. However, if the issue occurs on the latest version of the software, please file a [new bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md) with your full versioning information and full build log output.

### [IPA file is 0 bytes](ipa-zero-bytes.md)
There were some known issues in previous versions of Xamarin that could cause the IPA file on Windows to be 0 bytes.

### [IBTool Error: The operation couldn’t be completed.](error-ibtool.md)
Apple fixed this `ibtool` bug in Xcode 6.1.1, so upgrading to Xcode 6.1.1 or higher is the easiest fix.

### [Error MT1009: Could not copy the assembly](error-mt1009.md)
This affects users running Xamarin.iOS 7.2.6. This issue is due to file permissions needing higher privileges when Xamarin.iOS is installed with a different user account then the developer's main account.

### [System.Exception AMDeviceNotificationSubscribe returned ...](exception-amddevicenotificationsubscribe.md)
This message can appear in an error dialog when you first start Visual Studio for Mac, or in the `mtbserver.log` file. Note that this is an uncommon problem. If Visual Studio is having trouble connecting to the Mac build host, there are other errors that are more likely to appear in the `mtbserver.log` file.

### [MDocArchiveToMsxDocConverter.exe not found rver.BaseCommand.OnRequest](mdocarchivetomsxdocconverter-not-found.md)
This error may appear in the `Mac Server Log` in Visual Studio.