---
title: "Consuming and Creating Xamarin.Forms Plugins"
description: "This article explains how to consume and create Xamarin.Forms Plugins. Plugins are typically used to easily expose native platform features."
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2016
---

# Consuming and Creating Xamarin.Forms Plugins

There are many native platform features that exist across all platforms but
have slightly different APIs. Developers write plugins to create an abstract
cross-platform interface for those features that they can also share with others.

These features include: battery status, compass, motion sensors, geolocation,
text-to-speech, and a lot more. Plugins allow these features to be easily
accessed by Xamarin.Forms applications.

## Finding and Adding Plugins

The Xamarin community has created many cross-platform plugins compatible with
Xamarin.Forms - a large collection can be found at:

[**Xamarin Plugins**](https://github.com/xamarin/plugins)

For a guide to adding NuGet packages to your project, see our walkthrough on
[including a NuGet package in your project](/visualstudio/mac/nuget-walkthrough/).


## Creating plugins

It's also possible to create and publish your own plugins as Nuget packages
(and Xamarin Components). Many existing plugins are open-source so you can
review their code to understand how they have been writtern.

For example, the list of plugins below are all open source, and they correspond
to the samples in the [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
section:

- **Text-to-Speech** by James Montemagno &ndash;  [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/TextToSpeech) and [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)
- **Battery Status** by James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/Xamarin.Plugins/tree/master/Battery) and [NuGet](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech/)

Those Github projects can provide a good starting point for creating
your own cross-platform plugins, as do these instructions for
[creating a plugin for Xamarin](https://github.com/xamarin/plugins#create-a-plugin-for-xamarin).

### Structuring Cross-Platform Plugin Projects

Although there are no particular requirements for designing a NuGet package,
there are some guidelines for creating a package for cross-platform apps.

A cross-platform plugin should generally consist of the following components:

- PCL with an Interface that represents the API for the plugin,
- iOS, Android, and Windows class libraries with an implementation of the Interface.

Read James Montemagno's [blog post](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/)
describing the process of creating plugins for Xamarin.

It is a preferable to avoid referencing Xamarin.Forms directly from a plug-in.
This can create version-conflict issues when other developers attempt to use
the plug-in. Instead try to design the API so that it can be used by
any Xamarin or .NET application.

### Publishing NuGet Packages

NuGet packages have a **nuspec** file, which is an xml file that defines which
parts of your project are published in the package. The **nuspec** file also
includes information about the package, such as id, title, and authors.

See [NuGet's documentation](http://docs.nuget.org/create/creating-and-publishing-a-package)
for more information about creating and publishing NuGet packages.


## Related Links

- [Creating Reusable Plugins for Xamarin.Forms](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Using & Developing Plugins for Xamarin (video)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
