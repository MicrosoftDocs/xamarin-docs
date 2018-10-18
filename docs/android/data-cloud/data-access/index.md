---
title: "Xamarin.Android Data Access"
description: "Most applications have some requirement to save data on the device locally. Unless the amount of data is trivially small, this usually requires a database and a data layer in the application to manage database access.  Android has the SQLite database engine 'built in' and access to store and retrieve data is simplified by Xamarin's platform. This document shows how to access an SQLite database in a cross-platform way."
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
---

# Xamarin.Android Data Access

_Most applications have some requirement to save data on the device locally. Unless the amount of data is trivially small, this usually requires a database and a data layer in the application to manage database access.  Android has the SQLite database engine 'built in' and access to store and retrieve data is simplified by Xamarin's platform. This document shows how to access an SQLite database in a cross-platform way._

## Data Access Overview

Most applications have some requirement to save data on the device
locally. Unless the amount of data is trivially small, this usually
requires a database and a data layer in the application to manage
database access. Android both has the SQLite database engine "built
in" and access to the data is simplified by Xamarin's platform
which comes with the SQLite Data Provider.

Xamarin.Android support database access APIs such as:

- ADO.NET framework.
- SQLite-NET 3rd party library.

The majority of the code in this section is completely cross-platform
and will run on iOS or Android without modification. There are two
sample apps discussed:

- [**DataAccess_Basic**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
   &ndash; Simple data operations writes the results to a text display
   control;

- [**DataAccess_Advanced**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
   &ndash; Integrates data operations into a small working application that
   lists and edits a simple data structure.

Both sample solutions contain iOS and Android sample application
projects.

For Xamarin.Forms applications, read
[working with databases](~/xamarin-forms/app-fundamentals/databases.md)
which explains how to work with SQLite in a PCL library with
Xamarin.Forms.

The topics in this section discuss data access in Xamarin.Android using
SQLite as the database engine. The database can be accessed "directly"
by using ADO.NET syntax or you can include the SQLite.NET ORM and perform
data operations in C#.

Two samples are reviewed: one that contains very simple data access
code that outputs to a text field, and a simple application that
includes create, read, update and delete functionality. Threading and
how to seed your application with a pre-populated SQLite database is
also discussed.

For additional examples of cross-platform data access see our
[Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md)
case study.


## Related Links

- [DataAccess Basic (sample)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (sample)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android Data Recipes](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms data access](~/xamarin-forms/app-fundamentals/databases.md)
