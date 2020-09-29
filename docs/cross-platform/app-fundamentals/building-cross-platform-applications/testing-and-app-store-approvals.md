---
title: "Part 6 - Testing and App Store Approvals"
description: "This document describes how to test a cross-platform application on-device, manage test cases, automate tests, run unit tests, and work through the app submission process."
ms.prod: xamarin
ms.assetid: 46E0578A-7EB9-C105-ABB0-A043E501F36B
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
---

# Part 6 - Testing and App Store Approvals

## Testing

Many apps (even Android apps, on some stores) will have to pass an approval
process before they are published; so testing is critical to ensure your app
reaches the market (let alone succeeds with your customers). Testing can take
many forms, from developer-level unit testing to managing beta testing across a
wide variety of hardware.

### Test on All Platforms

There are slight differences between what .NET supports on Windows
phone, tablet, and desktop devices, as well as limitations on iOS that prevent dynamic code to
be generated on the fly. Either plan on testing the code on multiple platforms
as you develop it, or schedule time to refactor and update the model part of
your application at the end of the project.

It is always good practice to use the simulator/emulator to test multiple
versions of the operating system and also different device
capabilities/configurations.

You should also test on as many different physical hardware devices as you
can.

#### Devices in cloud

The mobile phone and tablet ecosystem is growing all the time, making it
impossible to test on the ever-increasing number of devices available. To solve
this problem a number of services offer the ability to remotely control many
different devices so that applications can be installed and tested without
needing to directly invest in lots of hardware.

App Center Test offers an easy way to test iOS and Android applications on hundreds of different devices. For more information, see [Preparing Xamarin.Android Apps](/appcenter/test-cloud/preparing-for-upload/xamarin-android-uitest) and [Preparing Xamarin.iOS apps](/appcenter/test-cloud/preparing-for-upload/xamarin-ios-uitest).

### Test Management

When testing applications within your organization or managing a beta program
with external users, there are two challenges:

- **Distribution** – Managing the provisioning process (especially for iOS devices) and getting updated versions of software to the testers.
- **Feedback** – Collecting information about application usage, and detailed information on any errors that may occur.

There are a number of services help to address these issues, by providing
infrastructure that is built into your application to collect and report on
usage and errors, and also streamlining the provisioning process to help sign-up
and manage testers and their devices.

[Visual Studio App Center](/appcenter/) offers a solution
to these issues, providing test version distribution, crash reporting, and sophisticated application
usage information.

### Test Automation

Xamarin [UITest](/appcenter/test-cloud/preparing-for-upload/uitest) can be used to create automated
user interface test scripts that can be run locally or uploaded to
[App Center Test](/appcenter/test-cloud/).

## Unit Testing

### Touch.Unit

Xamarin.iOS includes a unit-testing framework called Touch.Unit which follows
the JUnit/NUnit style writing tests.

Refer to our [Unit Testing with Xamarin.iOS](~/ios/deploy-test/touch.unit.md) documentation for details on writing tests and
running Touch.Unit.

### Andr.Unit

There is an open-source equivalent of Touch.Unit for Android called
Andr.Unit. You can download it from [github](https://github.com/spouliot/Andr.Unit) and read
about the tool on [@spouliot's blog](https://spouliot.wordpress.com/2011/10/30/andr-unit-joins-the-family/).

## App Store Approvals

Apple and Microsoft operate the only store on their platforms: the App Store
and Marketplace respectively. Both lock-down their devices and implement a
rigorous app review process to control the quality of applications available to
download. Android’s open nature means there are a number of store options
ranging from Google’s Play, which is widely available and has no review
process, to Amazon’s Appstore for Android and hardware-specific efforts like
Samsung Apps which have more limited distribution and implement an approval
process.

Waiting for an app to be reviewed can be very stressful - business pressures
often mean applications are submitted for approval with very little margin for
error prior to a “targeted” launch date. The process itself can take up to
two weeks and isn’t necessarily transparent: there is limited feedback on the
progress of your application until it is finally rejected or approved. Rejection
can mean missing a marketing window of opportunity, especially if it happens
more than once, and weeks pass between your original launch date and when the
app is finally approved.

### Be prepared

The first piece of advice: plan your app’s launch well in advance and make
allowances for a possible rejection and re-submission. Each store requires you
to create an account before submitting your app - do this as early as possible.
While Google Play’s signup only takes a few minutes if your apps are free, the
process gets a lot more involved if you are selling them and need to supply
banking and tax information. Apple and Microsoft’s processes are both much
more involved than Google’s, it could take a week or more to get your account
approved so factor this time into your launch plans.

Once your account has been approved, you’re ready to submit an app. The
actual process to submit apps is covered in the following documentation:

- [Publishing to Apple's iOS App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Preparing an app for Google Play](~/android/deploy-test/publishing/publishing-to-google-play/index.md)
- Windows developers should visit the [Windows Dev Center](https://developer.microsoft.com/windows/windows-apps) to read about submitting their apps.

The remainder of this section discusses things you should take into
consideration to ensure your app is approved without any hiccups.

### Quality

It sounds obvious, but applications will often get rejected because they do
not meet a certain level of quality: after all, this is the reason why the
curated stores have an approval process in the first place!

Crashes are a common reason for rejection. If it’s too easy to make your
app crash, it’s guaranteed to be rejected. Most developers don’t submit
their apps with the expectation that they’ll crash, yet they often do. Test
your app thoroughly before submitting it, focusing not just on making sure
everything works but also that you handle common mobile error scenarios such as
network problems and resource constraints like memory or storage space. Use both
the simulator and physical devices to test - regardless of how well code runs in
a simulator, only a device can demonstrate an app’s real performance. Use as
many different devices as you can find, and enlist a team of beta-testers if you
can - third-party services can help manage beta distribution and
feedback.

All mobile operating systems will kill an application that doesn’t start
quickly enough. The length of time allowed varies, but in general apps should
aim to be responsive in a few seconds and use background tasks to do any work
that would take longer. Apps that take too long to load, or are not responsive
enough in regular use will be rejected. Always provide user feedback when
something is happening in the background, or the app will appear to have crashed
and once again, get rejected.

### Check Your Edge Cases

A common trap for developers is failing to address edge-cases, especially
those that require re-configuring their simulator or device to test properly. It
can be easy to forget that not every customer is going to “Allow” your app
to access their location because after the developer has accepted the request
once, they’ll never be prompted again. Permissions and network usage are
specifically focussed on during the approval process, which means a small
oversight in these areas can result in rejection.

The following list is a good starting point for checking edge-cases that
might have been missed:

- **Customers may ‘deny’ access to services** – especially in iOS, access to data such as geo-location information is only provided if the user grants permission to your application. Application testers should frequently re-install the application in its initial state and disallow any permission requests to ensure the application behaves appropriately. Toggle permission on and off to verify correct behavior as customers change their mind.
- **Customers are everywhere** – don’t assume that an app will only be used in the city or country where it was developed! Code that works with GPS coordinates, date and time values and currencies can all be affected by the customer’s location and locale settings. All platforms offer a simulator that let you specify different locations and locales - use it to test locations in other hemispheres and with cultures that format dates and currencies differently. Latitude and longitude values can be positive or negative, the decimal separator could be a period or a comma, and dates can be formatted a myriad of ways - deal with it!
- **Slow network connections** – app developers often work in an ‘ideal world’ of fast, always working network connectivity, which obviously isn’t the case in the real world. Testing with slow network connectivity (such as a poor 3G connection) as well as with no network access is critical to ensuring you don’t ship a buggy app. The approval process will always include a test with the device in airplane mode, so ensure that you have tested that for yourself.
- **Hardware varies** – remember to test on the oldest, slowest hardware that you plan to support. There are two aspects that might affect your app: performance, which might be unusable on an older device, and support for hardware features such as a camera, microphone, GPS, gyroscope or other optional component. Applications should degrade gracefully (and not crash) when a component is unavailable.

### Guidelines are more than just a ‘guide’

Apple is famous for being strict about adherence to their Human Interface
Guidelines as one of the key strengths of their platform is consistency (and the
perceived increase in usability). Microsoft has taken a similar approach with
Windows applications implementing the [Fluent Design System](https://microsoft.com/design/fluent). The approval process
for both platforms will involve your app being evaluated for its adherence to
the relevant design philosophy.

That isn’t to say that user interface innovation isn’t supported or
encouraged, but there are certain things you “just shouldn’t do” or your
app will be rejected.

On iOS, this includes misusing built-in icons or using other well established
metaphors in a non-consistent manner; for example using the ‘compose’ icon
for anything other than creating new content.

Windows developers should be similarly careful; a common mistake is
failing to properly support the hardware Back button according to Microsoft’s
guidelines.

Encourage your designers to read and follow the design guidelines for each
platform.

### Implementing Platform-Specific Features

Things are a little stricter when it comes to implementing platform-specific
services, especially on iOS. To avoid automatic-rejection by Apple, there are
some rules to follow with the following iOS features:

- **In-App purchases** – Applications must NOT implement external payment mechanisms for digital products including in-game currency, application features, magazine subscriptions and a lot more. iOS apps must use Apple’s iTunes-based service for this kind of functionality. There is a loophole - apps like the Kindle Reader and some subscription-based apps let you purchase content elsewhere that gets attached to an “account” which you can then access via the app, however in this case the app must not contain links or references to the out-of-app purchase process (or, once again, it’ll be rejected).
- **iCloud backup** – With the advent of iCloud, Apple’s reviewers are much more strict regarding how apps use storage (to ensure customer’s remote backup experience is pleasant). Apps that waste backup-able storage space may get rejected, so use the Cache folder appropriately and follow Apple’s other storage-related guidelines.
- **Newsstand** – Newspaper and magazine apps are a great fit for Apple’s Newsstand, however apps must implement at least one auto-renewing subscription and support background downloading to be approved.
- **Maps** – It’s increasingly common to add overlays and other features to mobile maps, however be careful not to obscure the map ‘credits’ information (such as the Google logo in iOS5) as doing so will result in rejection.

### Manage Your Metadata

In addition to the obvious technical issues that can result in an application
being rejected, there are some more subtle aspects of your submission that could
result in rejection, especially around the metadata (description, keywords and
marketing images) that you submit with your application for display within the
App Store or Marketplace.

- **Imagery** – Follow the platform’s guidelines for application icons and store pictures. Don’t use trademarked images, we’ve seen apps get rejected because their icons featured a drawing of an iPhone!
- **Trademarks** – Avoid using any trademarks other than your own. Apps have been denied for mentioning trademarks in the app description or even in the keywords on Apple’s App Store.
- **Description** – Do not use the word ‘beta’ or in any way indicate that the app is not ready for prime time. Don’t mention other mobile platforms (even if your app is cross-platform). Most importantly, make sure the app does exactly what you say it does. If you list a bunch of features in your description, it had better be obvious how to use each of those features or you'll get a "feature mentioned in the application's description is not implemented" rejection.

Put as much effort into the application’s metadata as into development and
testing. Applications DO get rejected for minor infringements in the metadata so
it is worthwhile taking the time to get it right.

### App Stores: Not For Everyone

The primary focus of the stores on each platform is consumer distribution -
the ability to reach as many customers as possible. However not all applications
are targeted at consumers, there is a rapidly growing base of in-house and
extranet-like applications that require limited distribution to employees,
suppliers or customers. These apps aren’t “for sale” and don’t need
approval, since the developer controls distribution to a closed group of users.
Support for this type of deployment varies by platform.

Android offers the most flexibility in this regard: applications can be
installed directly from a URL or email attachment (so long as the device’s
configuration allows it). This means it is trivial to create and distribute
in-house corporate applications or publish applications to specific customers or
suppliers.

Apple provides an in-house deployment option to developers enrolled in the
iOS Developer Enterprise Program, which bypasses the App Store approval process
and allows companies to distribute in-house apps to their employees.
Unfortunately this license does not address the need for extranet-like app
distribution to other closed groups of customers or suppliers. [Enterprise (and Ad-Hoc) Deployment](~/ios/deploy-test/app-distribution/ipa-support.md)

### App Store Summary

The review process can be daunting, but like the rest of the development
lifecycle you can help ensure success with some planning and attention to
detail. It all comes down to a few simple steps: read and understand the user
interface guidelines you must adhere to, follow the rules if you are
implementing platform-specific features, test thoroughly (then test some more)
and finally make sure your application metadata is correct before you
submit.

One last word of advice to developers publishing on Google Play: the lack of
approval process may seem like it makes your job easier - but your customers
will be even more demanding than a review team. Follow these guidelines as
though your app could get rejected, otherwise it will be your customers doing
the rejecting.