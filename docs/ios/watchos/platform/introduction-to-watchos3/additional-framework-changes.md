---
title: "Additional watchOS 3 Frameworks Changes"
description: "This document describes various framework changes introduced with watchOS 3, and how to work with them in Xamarin. Core Data, Core Motion, Foundation, HealthKit, HomeKit, PassKit, and UIKit are discussed."
ms.service: xamarin
ms.assetid: FE93796E-F699-4B14-B37D-D39F9D48E81E
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
no-loc: [Objective-C]
---

# Additional watchOS 3 Frameworks Changes

_This article covers additional, minor changes or enhancements to existing frameworks for watchOS 3._

In addition to the major changes to iOS, Apple has made modifications and improvements to several existing frameworks in watchOS 3.

## Core Data

The following enhancements have be made to the Core Data framework for watch OS 3:

- Root [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objects supports concurrent faulting and fetching without serialization.
- The [NSPersistentStoreCoordinator](https://developer.apple.com/reference/coredata/nspersistentstorecoordinator) class maintains a pool of SQLite data stores.
- The [NSManagedObjectContext](https://developer.apple.com/reference/coredata/nsmanagedobjectcontext) objects with SQLite data stores in the WAL Journal Mode support the new query generation feature where Managed Object Contexts (MOC) can be pinned to specific database versions for future fetching and faulting transactions.
- Using the high-level `NSPersistenceContainer` to reference the `NSPersistentStoreCoordinator`, [NSManagedObjectModel](https://developer.apple.com/reference/coredata/nsmanagedobjectmodel) and other Core Data configuration resources.
- Several new convenience methods have been added to `NSManagedObject` making it easier to perform fetches and create subclasses.

For more information, please see Apple's [Core Data Framework Reference](https://developer.apple.com/reference/coredata).

## Core Motion

The following enhancements have be made to the Core Motion framework for watch OS 3:

- The new Device Motion event uses the accelerometer and gyroscope to provide motion and orientation updates. The app can register for this update (at rates of up to 100Hz).
- The new Pedometer event enables fast, real-time notifications when the user pauses and resumes running. Use the [CMPedometer](https://developer.apple.com/reference/coremotion/cmpedometer) to register for foreground or background pedometer events.

## Foundation

The following enhancements have be made to the Foundation framework for watch OS 3:

- Use the new [NSDateInterval](https://developer.apple.com/reference/foundation/nsdateinterval) class to make date and time interval calculations such as durations, for comparing intervals and testing for interval intersections.
- Several new properties have been added to the [NSLocal](https://developer.apple.com/reference/foundation/nslocale) class to acquire local information and the available display formats.
- Use the new [NSMeasurement](https://developer.apple.com/reference/foundation/nsmeasurement) class to convert between different Units of Measure (UOM) or perform calculations on values in different UOMs.
- Use the new [NSMeasurementFormatter](https://developer.apple.com/reference/foundation/nsmeasurementformatter) class to format localized measurements for displaying to the end user.
- Use the new [NSUnit](https://developer.apple.com/reference/foundation/nsunit) and [NSDimension](https://developer.apple.com/reference/foundation/nsdimension) classes for representing specific UOMs.

## HealthKit

The following enhancements have be made to the HealthKit framework for watch OS 3:

- Use the new [HKWorkoutConfiguration](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) class to specify the `ActivityType` and `LocationType` of a workout.
- The new [HKWheelchairUseObject](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) and the `WheelchairUse` method of the [HKHealthStore](https://developer.apple.com/reference/healthkit/hkhealthstore) class have been added for working with wheelchair related health data.
- New metadata keys have been added for weather types (such as `HKWeatherConditionClear` and `HKWeatherConditionCloudy`) and workout types (such as `HKWorkoutActivityTypeFlexibility` and `HKWorkoutActivityTypeWheelchairRunPace`) have been added.

## HomeKit

The following enhancements have be made to the HomeKit framework for watch OS 3:

- Added the ability to view and interact with HomeKit connected IP cameras.
- Added several new services and characteristics.
- Added more context and configuration of the accessories of primary services and link services.

## PassKit

The following enhancements have be made to the PassKit framework for watch OS 3:

- Expands the framework to support secure, in-app payments on the Apple Watch of both physical goods and services.
- The following classes are now available: [PKPayment](https://developer.apple.com/reference/passkit/pkpayment), [PKPaymentMethod](https://developer.apple.com/reference/passkit/pkpaymentmethod), [PKPaymentRequest](https://developer.apple.com/reference/passkit/pkpaymentrequest) and [PKPaymentToken](https://developer.apple.com/reference/passkit/pkpaymenttoken)

## UIKit

The following enhancements have be made to the UIKit framework for watch OS 3:

- To support Dynamic Type in labels, text fields and text boxes use the new `PreferredFontForTextStyle` method of the `UIFont` class.
- The `ColorWithDisplayP3` method was added to support Wide Color.

## Related Links

- [watchOS samples](/samples/browse/?products=xamarin&term=Xamarin.iOS%2bwatchos)
- [What's new in watchOS 3](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)