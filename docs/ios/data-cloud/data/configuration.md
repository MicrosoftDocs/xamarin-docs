---
title: "Configuring SQLite in Xamarin.iOS"
description: "This document describes how to determine the location for a SQLite database file in a Xamarin.iOS application. These concepts are relevant no matter the selected data access mechanism."
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/11/2016
---

# Configuring SQLite in Xamarin.iOS

To use SQLite in your Xamarin.iOS application you will need to determine the correct file location for your database file.

## Database File Path

Regardless of which data access method you use, you must create a database file before data can be stored with SQLite. Depending on what platform you are targeting the file location will be different. For iOS you can use Environment class to construct a valid path, as shown in the following code snippet:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

There are other things to take into consideration when deciding where to store the database file. On iOS you may want the database to backed-up automatically (or not).

If you wish to use a different location on each platform in your cross platform application you can use a compiler directive as shown to generate a different path for each platform:

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Refer to the [Working with the File System](~/ios/app-fundamentals/file-system.md) article for more information on what file locations to use on iOS. See the [Building Cross Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) document for more information on using compiler directives to write code specific to each platform.

## Threading

You should not use the same SQLite database connection across multiple threads. Be careful to open, use and then close any connections you create on the same thread.

To ensure that your code is not attempting to access the SQLite database from multiple threads at the same time, manually take a lock whenever you are going to access the database, like this:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

All database access (reads, writes, updates, etc) should be wrapped with the same lock. Care must be taken to avoid a deadlock situation by ensuring that the work inside the lock clause is kept simple and does not call out to other methods that may also take a lock!


## Related Links

- [DataAccess Basic (sample)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess Advanced (sample)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS Data Recipes](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms data access](~/xamarin-forms/app-fundamentals/databases.md)
