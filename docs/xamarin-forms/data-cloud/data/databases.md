---
title: "Xamarin.Forms Local Databases"
description: "Xamarin.Forms supports database-driven applications using the SQLite database engine, which makes it possible to load and save objects in shared code. This article describes how Xamarin.Forms applications can read and write data to a local SQLite database using SQLite.Net."
ms.service: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.subservice: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 03/01/2021
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Local Databases

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/todo)

The SQLite database engine allows Xamarin.Forms applications to load and save data objects in shared code. The sample application uses a SQLite database table to store todo items. This article describes how to use SQLite.Net in shared code to store and retrieve information in a local database.

[![Screenshots of the Todolist app on iOS and Android](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "Todolist app on iOS and Android")

Integrate SQLite.NET into mobile apps by following these steps:

1. [Install the NuGet package](#install-the-sqlite-nuget-package).
1. [Configure constants](#configure-app-constants).
1. [Create a database access class](#create-a-database-access-class).
1. [Access data in Xamarin.Forms](#access-data-in-xamarinforms).
1. [Advanced configuration](#advanced-configuration).

## Install the SQLite NuGet package

Use the NuGet package manager to search for **sqlite-net-pcl** and add the latest version to the shared code project.

There are a number of NuGet packages with similar names. The correct package has these attributes:

- **ID:** sqlite-net-pcl
- **Authors:** SQLite-net
- **Owners:** praeclarum
- **NuGet link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

Despite the package name, use the **sqlite-net-pcl** NuGet package even in .NET Standard projects.

 > [!IMPORTANT]
 > SQLite.NET is a third-party library that's supported from the [praeclarum/sqlite-net repo](https://github.com/praeclarum/sqlite-net).

## Configure app constants

The sample project includes a **Constants.cs** file that provides common configuration data:

```csharp
public static class Constants
{
    public const string DatabaseFilename = "TodoSQLite.db3";

    public const SQLite.SQLiteOpenFlags Flags =
        // open the database in read/write mode
        SQLite.SQLiteOpenFlags.ReadWrite |
        // create the database if it doesn't exist
        SQLite.SQLiteOpenFlags.Create |
        // enable multi-threaded database access
        SQLite.SQLiteOpenFlags.SharedCache;

    public static string DatabasePath
    {
        get
        {
            var basePath = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
            return Path.Combine(basePath, DatabaseFilename);
        }
    }
}
```

The constants file specifies default `SQLiteOpenFlag` enum values that are used to initialize the database connection. The `SQLiteOpenFlag` enum supports these values:

- `Create`: The connection will automatically create the database file if it doesn't exist.
- `FullMutex`: The connection is opened in serialized threading mode.
- `NoMutex`: The connection is opened in multi-threading mode.
- `PrivateCache`: The connection will not participate in the shared cache, even if it's enabled.
- `ReadWrite`: The connection can read and write data.
- `SharedCache`: The connection will participate in the shared cache, if it's enabled.
- `ProtectionComplete`: The file is encrypted and inaccessible while the device is locked.
- `ProtectionCompleteUnlessOpen`: The file is encrypted until it's opened but is then accessible even if the user locks the device.
- `ProtectionCompleteUntilFirstUserAuthentication`: The file is encrypted until after the user has booted and unlocked the device.
- `ProtectionNone`: The database file isn't encrypted.

You may need to specify different flags depending on how your database will be used. For more information about `SQLiteOpenFlags`, see [Opening A New Database Connection](https://www.sqlite.org/c3ref/open.html) on sqlite.org.

## Create a database access class

A database wrapper class abstracts the data access layer from the rest of the app. This class centralizes query logic and simplifies the management of database initialization, making it easier to refactor or expand data operations as the app grows. The Todo app defines a `TodoItemDatabase` class for this purpose.

### Lazy initialization

The `TodoItemDatabase` uses asynchronous lazy initialization, represented by the custom `AsyncLazy<T>` class, to delay initialization of the database until it's first accessed:

```csharp
public class TodoItemDatabase
{
    static SQLiteAsyncConnection Database;

    public static readonly AsyncLazy<TodoItemDatabase> Instance = new AsyncLazy<TodoItemDatabase>(async () =>
    {
        var instance = new TodoItemDatabase();
        CreateTableResult result = await Database.CreateTableAsync<TodoItem>();
        return instance;
    });

    public TodoItemDatabase()
    {
        Database = new SQLiteAsyncConnection(Constants.DatabasePath, Constants.Flags);
    }

    //...
}
```

The `Instance` field is used to create the database table for the `TodoItem` object, if it doesn't already exist, and returns a `TodoItemDatabase` as a singleton. The `Instance` field, of type `AsyncLazy<TodoItemDatabase>` is constructed the first time it's awaited. If multiple threads attempt to access the field simultaneously, they will all use the single construction. Then, when the construction completes, all `await` operations complete. In addition, any `await` operations after the construction is complete continue immediately since the value is available.

> [!NOTE]
> The database connection is a static field which ensures that a single database connection is used for the life of the app. Using a persistent, static connection offers better performance than opening and closing connections multiple times during a single app session.

### Asynchronous lazy initialization

In order to start the database initialization, avoid blocking execution, and have the opportunity to catch exceptions, the sample application uses asynchronous lazy initalization, represented by the `AsyncLazy<T>` class:

```csharp
public class AsyncLazy<T>
{
    readonly Lazy<Task<T>> instance;

    public AsyncLazy(Func<T> factory)
    {
        instance = new Lazy<Task<T>>(() => Task.Run(factory));
    }

    public AsyncLazy(Func<Task<T>> factory)
    {
        instance = new Lazy<Task<T>>(() => Task.Run(factory));
    }

    public TaskAwaiter<T> GetAwaiter()
    {
        return instance.Value.GetAwaiter();
    }
}
```

The `AsyncLazy` class combines the `Lazy<T>` and `Task<T>` types to create a lazy-initialized task that represents the initialization of a resource. The factory delegate that's passed to the constructor can either be synchronous or asynchronous. Factory delegates will run on a thread pool thread, and will not be executed more than once (even when multiple threads attempt to start them simultaneously). When a factory delegate completes, the lazy-initialized value is available, and any methods awaiting the `AsyncLazy<T>` instance receive the value. For more information, see [AsyncLazy](https://devblogs.microsoft.com/pfxteam/asynclazyt/).

### Data manipulation methods

The `TodoItemDatabase` class includes methods for the four types of data manipulation: create, read, edit, and delete. The SQLite.NET library provides a simple Object Relational Map (ORM) that allows you to store and retrieve objects without writing SQL statements.

```csharp
public class TodoItemDatabase
{
    // ...
    public Task<List<TodoItem>> GetItemsAsync()
    {
        return Database.Table<TodoItem>().ToListAsync();
    }

    public Task<List<TodoItem>> GetItemsNotDoneAsync()
    {
        // SQL queries are also possible
        return Database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
    }

    public Task<TodoItem> GetItemAsync(int id)
    {
        return Database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
    }

    public Task<int> SaveItemAsync(TodoItem item)
    {
        if (item.ID != 0)
        {
            return Database.UpdateAsync(item);
        }
        else
        {
            return Database.InsertAsync(item);
        }
    }

    public Task<int> DeleteItemAsync(TodoItem item)
    {
        return Database.DeleteAsync(item);
    }
}
```

## Access data in Xamarin.Forms

The `TodoItemDatabase` class exposes the `Instance` field, through which the data access operations in the `TodoItemDatabase` class can be invoked:

```csharp
async void OnSaveClicked(object sender, EventArgs e)
{
    var todoItem = (TodoItem)BindingContext;
    TodoItemDatabase database = await TodoItemDatabase.Instance;
    await database.SaveItemAsync(todoItem);

    // Navigate backwards
    await Navigation.PopAsync();
}
```

## Advanced configuration

SQLite provides a robust API with more features than are covered in this article and the sample app. The following sections cover features that are important for scalability.

For more information, see [SQLite Documentation](https://www.sqlite.org/docs.html) on sqlite.org.

### Write-ahead logging

By default, SQLite uses a traditional rollback journal. A copy of the unchanged database content is written into a separate rollback file, then the changes are written directly to the database file. The COMMIT occurs when the rollback journal is deleted.

Write-Ahead Logging (WAL) writes changes into a separate WAL file first. In WAL mode, a COMMIT is a special record, appended to the WAL file, which allows multiple transactions to occur in a single WAL file. A WAL file is merged back into the database file in a special operation called a _checkpoint_.

WAL can be faster for local databases because readers and writers do not block each other, allowing read and write operations to be concurrent. However, WAL mode doesn't allow changes to the _page size_, adds additional file associations to the database, and adds the extra _checkpointing_ operation.

To enable WAL in SQLite.NET, call the `EnableWriteAheadLoggingAsync` method on the `SQLiteAsyncConnection` instance:

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

For more information, see [SQLite Write-Ahead Logging](https://www.sqlite.org/wal.html) on sqlite.org.

### Copy a database

There are several cases where it may be necessary to copy a SQLite database:

- A database has shipped with your application but must be copied or moved to writeable storage on the mobile device.
- You need to make a backup or copy of the database.
- You need to version, move, or rename the database file.

In general, moving, renaming, or copying a database file is the same process as any other file type with a few additional considerations:

- All database connections should be closed before attempting to move the database file.
- If you use [Write-Ahead Logging](#write-ahead-logging), SQLite will create a Shared Memory Access (.shm) file and a (Write Ahead Log) (.wal) file. Ensure that you apply any changes to these files as well.

For more information, see [File Handling in Xamarin.Forms](~/xamarin-forms/data-cloud/data/files.md).

## Related links

- [Todo sample application](/samples/xamarin/xamarin-forms-samples/todo)
- [SQLite.NET NuGet package](https://www.nuget.org/packages/sqlite-net-pcl/)
- [SQLite documentation](https://www.sqlite.org/docs.html)
- [Using SQLite with Android](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [Using SQLite with iOS](~/ios/data-cloud/data/using-sqlite-orm.md)
- [AsyncLazy](https://devblogs.microsoft.com/pfxteam/asynclazyt/)
