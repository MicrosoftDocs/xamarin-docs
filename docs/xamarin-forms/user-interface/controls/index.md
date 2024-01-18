---
title: "Controls Reference"
description: "A description of all the user interface elements used to construct a Xamarin.Forms application. This article lists the control groups that make up the user interface of a Xamarin.Forms application."
ms.service: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Controls Reference

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/formsgallery/)

The user interface of a Xamarin.Forms application is constructed of objects that map to the native controls of each target platform. This allows platform-specific applications for iOS, Android, and the Universal Windows Platform to use Xamarin.Forms code contained in a [.NET Standard library](~/cross-platform/app-fundamentals/net-standard.md).

The four main control groups used to create the user interface of a Xamarin.Forms application are as follows:

- [**Pages**](pages.md)
- [**Layouts**](layouts.md)
- [**Views**](views.md)
- [**Cells**](cells.md)

A Xamarin.Forms page generally occupies the entire screen. The page usually contains a layout, which contains views and possibly other layouts. Cells are specialized components used in connection with [`TableView`](xref:Xamarin.Forms.TableView) and [`ListView`](xref:Xamarin.Forms.ListView). A class diagram that shows the hierarchy of types that are typically used to build a user interface in Xamarin.Forms can be found at [Xamarin.Forms Controls Class Hierarchy](~/xamarin-forms/internals/class-hierarchy.md).

In the four articles on [**Pages**](pages.md), [**Layouts**](layouts.md), [**Views**](views.md), and [**Cells**](cells.md), each type of control is described with links to its API documentation, an article describing its use (if one exists), and one or more sample programs (if they exist). Each type of control is also accompanied by a screenshot showing a page from the [**FormsGallery**](/samples/xamarin/xamarin-forms-samples/formsgallery) sample running on iOS and Android devices. Below each screenshot are links to the source code for the C# page, the equivalent XAML page, and (when appropriate) the C# code-behind file for the XAML page.

> [!NOTE]
> Pages, Layouts, and Views derive from the `VisualElement` class. The `VisualElement` class provides a variety of properties, methods, and events that are useful in deriving classes. For more information, see [VisualElement properties, methods, and events](common-properties.md).

In addition to the controls supplied with Xamarin.Forms, third-party controls are available. For more information, see [Third Party Controls](thirdparty.md).

## Related Links

- [Xamarin.Forms FormsGallery sample](/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms Controls Class Hierarchy](~/xamarin-forms/internals/class-hierarchy.md)
- [API Documentation](/dotnet/api/xamarin.forms?view=xamarin-forms&preserve-view=true)