---
title: "Xamarin.Forms Frequently Asked Questions"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 89364175-53BA-4A09-B3E2-44AC67DD971C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Frequently Asked Questions

## [How do I migrate my app to Xamarin.Forms 5.0?](forms5-migration.md)

Migrating an app to Xamarin.Forms 5.0 requires knowledge of its breaking changes.

## [Can I update the Xamarin.Forms default template to a newer NuGet package?](update-forms-template.md)

This guide uses the Xamarin.Forms .NET Standard library template as an example, but the same general method will also work for the Xamarin.Forms Shared Project template.

## [Why doesn't the Visual Studio XAML designer work for Xamarin.Forms XAML files?](forms-xaml-designer.md)

Xamarin.Forms doesn't currently support visual designers for XAML files.

## [Android build error: The "LinkAssemblies" task failed unexpectedly](android-linkassemblies-error.md)

You may see an error message `The "LinkAssemblies" task failed unexpectedly` when building a Xamarin.Android project that uses Forms. This happens when the linker is active (typically on a *Release* build to reduce the size of the app package); and it occurs because the Android targets aren't updated to the latest framework.

## ["Why does my Xamarin.Forms.Maps Android project fail with COMPILETODALVIK : UNEXPECTED TOP-LEVEL ERROR?"](maps-compiletodalvik-error.md)

This error may be seen in the Error pad of Visual Studio for Mac or in the Build Output window of Visual Studio; in Android projects using Xamarin.Forms.Maps. It is most commonly resolved by increasing the Java Heap Size for your Xamarin.Android project.
