---
title: "Xamarin.Forms Shell Introduction"
description: "Xamarin.Forms Shell provides the fundamental features that most applications require, including a common navigation user experience, a URI-based navigation scheme, and an integrated search handler."
ms.prod: xamarin
ms.assetid: 4604DCB5-83DA-458A-8B02-6508A740BE0E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2019
---

# Xamarin.Forms Shell Introduction

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms Shell reduces the complexity of mobile application development by providing the fundamental features that most mobile applications require, including:

- A single place to describe the visual hierarchy of an application.
- A common navigation user experience.
- A URI-based navigation scheme that permits navigation to any page in the application.
- An integrated search handler.

In addition, Shell applications benefit from an increased rendering speed, and reduced memory consumption.

> [!IMPORTANT]
> Xamarin.Forms Shell is only available on iOS and Android. Existing iOS and Android applications can adopt Shell and benefit immediately from navigation, performance, and extensibility improvements.

## Shell navigation experience

Shell provides an opinionated navigation experience, based on flyouts and tabs. The top level of navigation in a Shell application is either a flyout or a bottom tab bar, depending on the navigation requirements of the application. The following example shows an application where the top level of navigation is a flyout:

[![Screenshot of a Shell flyout, on iOS and Android](introduction-images/flyout.png "Shell flyout")](introduction-images/flyout-large.png#lightbox "Shell flyout")

Selecting a flyout item results in the bottom tab that represents the item being selected and displayed:

[![Screenshot of Shell bottom tabs, on iOS and Android](introduction-images/monkeys.png "Shell bottom tabs")](introduction-images/monkeys-large.png#lightbox "Shell bottom tabs")

> [!NOTE]
> When the flyout isn't open the bottom tab bar can be considered to be the top level of navigation in the application.

Each tab displays a [`ContentPage`](xref:Xamarin.Forms.ContentPage). However, if a bottom tab contains more than one page, the pages are navigable by the top tab bar:

[![Screenshot of Shell top tabs, on iOS and Android](introduction-images/cats.png "Shell top tabs")](introduction-images/cats-large.png#lightbox "Shell top tabs")

Within each tab, additional [`ContentPage`](xref:Xamarin.Forms.ContentPage) objects can be navigated to:

[![Screenshot of Shell page navigation, on iOS and Android](introduction-images/cat-details.png "Shell app navigation")](introduction-images/cat-details-large.png#lightbox "Shell app navigation")

## Related links

- [Xaminals (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
