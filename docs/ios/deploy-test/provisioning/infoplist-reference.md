---
title: "Info.plist Reference"
ms.topic: article
ms.prod: xamarin
ms.assetid: 944DFDB5-ADBA-4D6E-984C-5AEC19A1CC57
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/18/2017
---

# Info.plist Reference

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
