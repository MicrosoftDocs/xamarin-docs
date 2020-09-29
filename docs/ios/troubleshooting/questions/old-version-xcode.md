---
title: "Can I use an older version of Xcode or Xamarin.iOS"
description: "This guide outlines the issues with using older versions of Xamarin.iOS or Xcode (than the current stable release)."
ms.prod: xamarin
ms.assetid: 27CF7EB7-9251-435F-BEA5-F20F8DD7DC17
ms.technology: xamarin-ios
author: chamons
ms.author: chhamo
ms.date: 04/16/2019
---

# Can I use an older version of Xcode or Xamarin.iOS?

Xamarin documentation assumes use of the latest Xamarin.iOS and Xcode, which is recommended. However, some customers would prefer to use older Xamarin.iOS and/or Xcode, and would like details on the consequences.

The release notes contain the following warning:

> [!WARNING]
> **Using an older Xcode version**
>
> Using an older Xcode version (than the one mentioned in the above [requirements](/xamarin/ios/release-notes/12/12.8#requirements)) is often possible, but some features may not be available. Also some limitations might require workarounds, e.g.:
>
> - The static registrar requires Xcode headers files to build applications, leading to [`MT0091`](../mtouch-errors.md#MT0091) or [`MT4109`](../mtouch-errors.md#MT4109) errors if APIs are missing. In most cases enabling the managed linker will help (by removing the API).
> - Bitcode builds (for tvOS and watchOS) can fail submission to the App Store unless an Xcode 9.0+ toolchain is used.

## Further information

Microsoft strongly recommends using the latest Xcode and most recent Xamarin.iOS release when developing and submitting applications. Apple requires using the most recent Xcode when submitting applications.

Note that using the latest Xcode does not prevent your application from targeting older iOS versions. The iOS versions you support is based upon your **Info.plist** entry and the APIs your application uses.

It is possible to install multiple versions of Xcode side-by-side, with different names such as **Xcode101.app** and **Xcode102.app**. If you use multiple versions, make sure to set the active Xcode in [Visual Studio for Mac Settings](~/ios/troubleshooting/questions/ios-sdk.md) and with the [`xcode-select`](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_) [command line tool](https://developer.apple.com/library/archive/technotes/tn2339/_index.html#//apple_ref/doc/uid/DTS40014588-CH1-HOW_DO_I_SELECT_THE_DEFAULT_VERSION_OF_XCODE_TO_USE_FOR_MY_COMMAND_LINE_TOOLS_).

However, rare circumstances may require use of older components. This documentation will describe the general challenges you may face when using versions older than latest.

Each release from Apple is unique though, and you may come across other pitfalls not documented here.

These challenges are sometimes non-trivial to solve, so whenever possible stick to the supported configuration of latest Xcode and latest Xamarin.iOS.

## Use of an old Xamarin.iOS with an old Xcode

Not updating Xamarin.iOS and Xcode is possible, at least for some amount of time. The limit is that, at some point, Apple will require a minimum version of Xcode to submit your applications. At this point you should update all your components (macOS, Xcode and Xamarin.iOS) to the latest versions (or the new, minimal version of Xcode required by Apple and the matching Xamarin.iOS release).

It’s generally easier to gradually update and keep up with the small changes. For large projects where updates can be harder to keep up with, staying with known working set can be a good compromise.

## Use of new Xamarin.iOS with older Xcode

Xamarin.iOS in general supports older Xcode releases whenever reasonably possible. A few potential challenges include:

- The newer Xamarin.iOS may support some features and APIs not present in the selected Xcode. 
- The **static registrar** requires Xcode headers files to build applications, leading to [`MT0091`](~/ios/troubleshooting/mtouch-errors.md#MT0091) or [`MT4109`](~/ios/troubleshooting/mtouch-errors.md#MT4109) errors if APIs are missing.
  - In most cases enabling the managed linker will help (by removing the managed bindings for the new API) if unused.
- Bitcode builds (for tvOS and watchOS) can fail submission to the App Store unless an Xcode 9.0+ toolchain is used.

## Use of new Xcode with older Xamarin.iOS

This use case is significantly more difficult, as Xamarin.iOS can not predict the changing requirements of new Xcode. Updates of macOS can also introduce problems, and without compatibility patches many parts of Xamarin.iOS could be affected. 

There are a number of potential areas where things can go wrong including:

- Incompatibilities with `mlaunch`:
  - Simulator support might not work properly (or at all)
  - Device support might not work properly (or at all)
- Unknown support for `mtouch` 
  - No support for new frameworks
  - No support for new entitlements
  - No support for new or updated tools
    - This can affect code signing as well

### New AppStore submission rules

Apple reserves the right to updates to the AppStore submissions rules at any time. These rule changes are only sometimes announced in advance. Some of these changes require tooling changes to support, which would require an updated Xamarin.iOS component.

In addition to the rule changes, Apple often adds additional validations to submitted apps or tightens existing ones. Some of these require changes in our tools (e.g. a new blacklisted symbols). Many of these are first encountered by customers submitting, as there is no announcement (or list) of the rules.

## Summary

Whenever possible, play it safe by following Apple’s guidance and developing on and submitting with the latest Xcode released on the App Store.

In turn, use the latest Xamarin.iOS released. This will track the latest fixes that may affect submitted applications and comply with the most recent rule changes.

Where that is not feasible, consider using a matched older Xcode and Xamarin.iOS release. This  can work for a time, but at some point Apple will insist upon newer tooling so plan accordingly.