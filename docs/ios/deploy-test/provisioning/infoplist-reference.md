---
title: "Info.plist Reference for Xamarin.iOS"
description: "This document describes various key/value pairs that can be set in the Info.plist file of a Xamarin.iOS application. These keys are necessary if your app performs specific tasks such as accessing location, photos, the microphone, or the camera."
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/18/2017
---

# Info.plist Reference for Xamarin.iOS

For more information on working with Info.Plist keys please refer to the [Working with Security and Privacy](~/ios/app-fundamentals/security-privacy.md) guide. 

## Location 

Accessing the user's location also requires modifications to Info.plist. The following keys relating to location data should be set: 

* **NSLocationWhenInUseUsageDescription** - For when you are accessing the user's location while they are interacting with your app. 
* **NSLocationAlwaysUsageDescription** - For when your app accesses the user's location in the background.

## Photos 

NSPhotoLibraryUsageDescription  

## Contacts 

NSContactsUsageDescription 

## Calendar data 
    
NSCalendarsUsageDescription 

## Reminder data 
    
NSRemindersUsageDescription 

## Bluetooth peripherals 
    
NSBluetoothPeripheralUsageDescription 

## Microphone 

NSMicrophoneUsageDescription 

## Camera 
    
NSCameraUsageDescription 

## Reading HealthKit  

NSHealthShareUsageDescription 

NSHealthUpdateUsageDescription 

## Background modes 
    
UIBackgroundModes 

## HomeKit 

NSHomeKitUsageDescription 


## Related Links

- [Apple Reference guide.](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW10)
