---
title: "Xamarin.Forms Data Binding"
description: "Data binding is the technique of linking properties of two objects so that changes in one property are automatically reflected in the other property. Data binding is an integral part of the Model-View-ViewModel (MVVM) application architecture."
ms.prod: xamarin
ms.assetid: 938E85C8-521D-43B9-92CB-D591A06D98A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/23/2018
---

# Xamarin.Forms Data Binding

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)

_Data binding is the technique of linking properties of two objects so that changes in one property are automatically reflected in the other property. Data binding is an integral part of the Model-View-ViewModel (MVVM) application architecture._

## The Data Linking Problem

A Xamarin.Forms application consists of one or more pages, each of which generally contains multiple user-interface objects called *views*. One of the primary tasks of the program is to keep these views synchronized, and to keep track of the various values or selections that they represent. Often the views represent values from an underlying data source, and the user manipulates these views to change that data. When the view changes, the underlying data must reflect that change, and similarly, when the underlying data changes, that change must be reflected in the view.

To handle this job successfully, the program must be notified of changes in these views or the underlying data. The common solution is to define events that signal when a change occurs. An event handler can then be installed that is notified of these changes. It responds by transferring data from one object to another. However, when there are many views, there must also be many event handlers, and a lot of code gets involved.

## The Data Binding Solution

Data binding automates this job, and renders the event handlers unnecessary. (The events are still necessary, however, because the data-binding infrastructure uses them.) Data bindings can be implemented either in code or in XAML, but they are much more common in XAML where they help to reduce the size of the code-behind file. By replacing procedural code in event handlers with declarative code or markup, the application is simplified and clarified.

One of the two objects involved in a data binding is almost always an element that derives from `View` and forms part of the visual interface of a page. The other object is either:

- Another `View` derivative, usually on the same page.
- An object in a code file.

In demonstration programs such as those in the [**DataBindingDemos**](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/) sample, data bindings between two `View` derivatives are often shown for purposes of clarity and simplicity. However, the same principles can be applied to data bindings between a `View` and other objects. When an application is built using the Model-View-ViewModel (MVVM) architecture, the class with underlying data is often called a ViewModel.

Data bindings are explored in the following series of articles:

## [Basic Bindings](basic-bindings.md)

Learn the difference between the data binding target and source, and see simple data bindings in code and XAML.

## [Binding Mode](binding-mode.md)

Discover how the binding mode can control the flow of data between the two objects.

## [String Formatting](string-formatting.md)

Use a data binding to format and display objects as strings.

## [Binding Path](binding-path.md)

Dive deeper into the `Path` property of the data binding to access sub-properties and collection members.

## [Binding Value Converters](converters.md)

Use binding value converters to alter values within the data binding.

## [Binding Fallbacks](binding-fallbacks.md)

Make data bindings more robust by defining fallback values to use if the binding process fails.

## [The Command Interface](commanding.md)

Implement the `Command` property with data bindings.

## [Compiled Bindings](compiled-bindings.md)

Use compiled bindings to improve data binding performance.

## Related Links

- [Data Binding Demos (sample)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Data binding chapter from Xamarin.Forms book](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
- [XAML Markup Extensions](~/xamarin-forms/xaml/markup-extensions/index.md)
