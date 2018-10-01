---
title: "Xamarin.iOS Data Access"
description: "This document links to guides that describe how to work with local databases in a Xamarin.iOS application. Linked content discusses SQLite.NET, ADO.NET, and more."
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/11/2016
---

# Xamarin.iOS Data Access

Xamarin.iOS supports database access APIs such as:

-  ADO.NET framework.
-  SQLite-NET 3rd party library.

This guide provides a high-level overview of databases in general before describing how to set up ADO.NET and SQLite.NET to access SQLite databases in your Xamarin.iOS applications. 

The majority of the code in this document is completely cross-platform and will run on iOS or Android without modification. There are two sample apps discussed:

-  [**DataAccess_Basic**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) –
  Simple data operations writes the results to a text display control;
-  [**DataAccess_Advanced**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) –
  Integrates data operations into a small working application that lists and edits a simple data structure.

Both sample solutions contain iOS and Android sample application projects.

For Xamarin.Forms applications, read [working with databases](~/xamarin-forms/app-fundamentals/databases.md)
which explains how to work with SQLite in a PCL library with Xamarin.Forms.

## Sections

-  [Introduction](introduction.md)
-  [Configuration](configuration.md)
-  [Using SQLite.NET ORM](using-sqlite-orm.md)
-  [Using ADO.NET](using-adonet.md)
-  [Using Data in an App](using-data-in-an-app.md)

## Summary

This chapter discussed data access in Xamarin.iOS using SQLite as the database engine. The database can be accessed “directly” using ADO.NET syntax or you can include the SQLite.NET ORM and perform data operations in C#.

We reviewed two samples: one that contains very simple data access code that outputs to a text field, and a simple application that includes create, read, update and delete functionality. We also discussed threading and how to seed your application with a pre-populated SQLite database.

For additional examples of cross-platform data access see our [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) case study.

## Related Links

- [DataAccess Basic (sample)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (sample)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS Data Recipes](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms data access](~/xamarin-forms/app-fundamentals/databases.md)
