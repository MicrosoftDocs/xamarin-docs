---
title: "DataPages"
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
---

# DataPages

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages requires a
[Xamarin.Forms Theme](~/xamarin-forms/user-interface/themes/index.md) reference to render.

Xamarin.Forms DataPages were announced at Evolve 2016 and are available as a
preview for customers to try and provide feedback.

DataPages provide an API to quickly and easily bind a data source
to pre-built views. List items and detail pages
will automatically render the data, and can be customized using themes.

To see how the Evolve keynote demo works, check out the
[getting started guide](get-started.md).

[![](images/demo-sml.png "DataPages Sample Application")](images/demo.png#lightbox "DataPages Sample Application")

## Introduction

Data sources and the associated data pages allow developers to quickly
and easily consume a supported data source and render it using built-in
UI scaffolding that can be customized with themes.

DataPages are added to a Xamarin.Forms application by including
the **Xamarin.Forms.Pages** Nuget package.

### Data Sources

The Preview has some prebuilt data sources available for use:

* **JsonDataSource**
* **AzureDataSource** (separate Nuget)
* **AzureEasyTableDataSource** (separate Nuget)

See the [getting started guide](get-started.md) for an example
using a `JsonDataSource`.


### Pages & Controls

The following pages and controls are included to allow easy binding
to the supplied data sources:

* **ListDataPage** – see the [getting started example](get-started.md).
* **DirectoryPage** – a list with grouping enabled.
* **PersonDetailPage** – a single data item view customized for a specific object type (a contact entry).
* **DataView** – a view to expose data from the source in a generic fashion.
* **CardView** – a styled view that contains an image, title text, and description text.
* **HeroImage** – an image-rendering view.
* **ListItem** – a pre-built view with a layout similar to native iOS and Android list items.

See the [DataPages controls reference](controls.md) for examples.



### Under the Hood

A Xamarin.Forms data source adheres to the `IDataSource` interface.

The Xamarin.Forms infrastructure interacts with a data source through
the following properties:

* `Data` – a read-only list of data items that can be displayed.
* `IsLoading` – a boolean indicating whether the data is loaded and available for rendering.
* `[key]` – an indexer to retrieve elements.

There are two methods `MaskKey` and `UnmaskKey` that can be used
to hide (or show) data item properties (ie. prevent them from being rendered).
The key corresponds to the a named property on the data item object.

