---
title: "Xamarin.Forms Data Templates"
description: "A DataTemplate is used to specify the appearance of data on supported controls, and typically binds to the data to be displayed."
ms.service: xamarin
ms.assetid: 838F4BDB-B719-457F-8633-27E9B267A2A0
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/11/2017
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Data Templates

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/templates-datatemplates)

_A DataTemplate is used to specify the appearance of data on supported controls, and typically binds to the data to be displayed._

## [Introduction](introduction.md)

Xamarin.Forms data templates provide the ability to define the presentation of data on supported controls. This article provides an introduction to data templates, examining why they are necessary.

## [Creating a DataTemplate](creating.md)

Data templates can be created inline, in a [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary), or from a custom type or appropriate Xamarin.Forms cell type. An inline template should be used if there's no need to reuse the data template elsewhere. Alternatively, a data template can be reused by defining it as a custom type, or as a control-level, page-level, or application-level resource.

## [Creating a DataTemplateSelector](selector.md)

A [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) can be used to choose a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) at runtime based on the value of a data-bound property. This enables multiple `DataTemplate` instances to be applied to the same type of object, to customize the appearance of particular objects. This article demonstrates how to create and consume a `DataTemplateSelector`.

## Related Links

- [Data Templates (sample)](/samples/xamarin/xamarin-forms-samples/templates-datatemplates)