---
title: "Consuming and Creating Xamarin.Forms Plugins"
description: "This article explains how to consume and create Xamarin.Forms Plugins. Plugins are typically used to easily expose native platform features."
ms.prod: xamarin
ms.assetid: 8A06A420-A9D0-4BCB-B9AF-3AEA6A648A8B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/05/2018
---

# Consuming and Creating Xamarin.Forms Plugins

There are many native platform features that exist across all platforms but
have slightly different APIs. One way for developers to use these features is by creating an abstract cross-platform interface, and then implementing that interface in the various platforms. The Xamarin.Forms application then accesses these platform implementations using [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

Developers can share this work by writing a _plugin_ and publishing it to NuGet.

> [!NOTE]
> Many cross-platform features previously available only through plugins are now part 
of the open-source **[Xamarin.Essentials](~/essentials/index.md)** library. These features include: battery status, compass, motion sensors, geolocation, text-to-speech, and a lot more. In the future, **Xamarin.Essentials** will be the primary source of cross-platform features for Xamarin.Forms applications. Although developers can still create and publish plugins, consider contributing to **Xamarin.Essentials**.

## Finding and Adding Plugins

The Xamarin community has created many cross-platform plugins compatible with
Xamarin.Forms. A large collection can be found at:

[**Xamarin Plugins**](https://github.com/xamarin/XamarinComponents)

For a guide to adding NuGet packages to your project, see our walkthrough on
[including a NuGet package in your project](/visualstudio/mac/nuget-walkthrough/).

## Creating plugins

It's also possible to create and publish your own plugins as Nuget packages
(and Xamarin Components). Many existing plugins are open-source so you can
review their code to understand how they have been writtern.

For example, the list of plugins below are all open source, and they correspond
to some samples in the [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md)
section:

- **Text-to-Speech** by James Montemagno &ndash;  [GitHub](https://github.com/jamesmontemagno/TextToSpeechPlugin) and [NuGet](https://www.nuget.org/packages/Xam.Plugins.TextToSpeech)
- **Battery Status** by James Montemagno &ndash; [GitHub](https://github.com/jamesmontemagno/BatteryPlugin) and [NuGet](https://www.nuget.org/packages/Xam.Plugin.Battery)

Those Github projects can provide a good starting point for creating
your own cross-platform plugins, as do these instructions for
[creating a plugin for Xamarin](https://github.com/xamarin/XamarinComponents#create-a-plugin-for-xamarin).

### Structuring Cross-Platform Plugin Projects

Although there are no particular requirements for designing a NuGet package,
there are some guidelines for creating a package for cross-platform apps.

In the past, a cross-platform plugin generally consisted of the following components:

- PCL with an Interface that represents the API for the plugin,
- iOS, Android, and Universal Windows Platform (UWP) class libraries with an implementation of the Interface.

Read James Montemagno's [blog post](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms/)
describing the process of creating plugins for Xamarin.

More recently, plugins can be created with a single multi-targeted platform. This approach is discussed in James Montemagno's [blog post](https://montemagno.com/converting-xamarin-libraries-to-sdk-style-multi-targeted-projects/). This approach is used in James Montemagno's plugins linked above, and is also the format used in **Xamarin.Essentials**.

It is a preferable to avoid referencing Xamarin.Forms directly from a plug-in.
This can create version-conflict issues when other developers attempt to use
the plug-in. Instead try to design the API so that it can be used by
any Xamarin or .NET application.

### Publishing NuGet Packages

NuGet packages have a **nuspec** file, which is an xml file that defines which
parts of your project are published in the package. The **nuspec** file also
includes information about the package, such as id, title, and authors.

See [NuGet's documentation](/nuget/create-packages/creating-a-package.md)
for more information about creating and publishing NuGet packages.

## Related Links

- [Creating Reusable Plugins for Xamarin.Forms](https://blog.xamarin.com/creating-reusable-plugins-for-xamarin-forms)
- [Using & Developing Plugins for Xamarin (video)](https://university.xamarin.com/guestlectures/using-developing-plugins-for-xamarin)
