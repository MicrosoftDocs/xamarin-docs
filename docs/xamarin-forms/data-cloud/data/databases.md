---
title: "Xamarin.Forms Local Databases"
description: "Xamarin.Forms supports database-driven applications using the SQLite database engine, which makes it possible to load and save objects in shared code. This article describes how Xamarin.Forms applications can read and write data to a local SQLite database using SQLite.Net."
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 12/05/2019
---

# Xamarin.Forms Local Databases

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

The SQLite database engine allows Xamarin.Forms applications to load and save objects in shared code. The sample application uses a SQLite database table to store todo items. This article describes how to use SQLite.Net in shared code to store and retrieve information in a local database.

[![Screenshots of the Todolist app on iOS and Android](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "Todolist app on iOS and Android")

Integrate SQLite.NEt into mobile apps by following these steps:

1. Install the **sqlite-net-pcl** NuGet package.
1. Configure app constants.
1. Create a database access class.
1. Store and retrieve data.
1. Configure advanced features.

## Install SQLite Nuget packages

Use the NuGet package manager to search for **sqlite-net-pcl" and add the latest version to the shared code project.

There are a number of NuGet packages with similar names. The correct package has these attributes:

- **Created by:** Frank A. Krueger
- **Id:** sqlite-net-pcl
- **NuGet link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Despite the package name, use the **sqlite-net-pcl** NuGet package even in .NET Standard projects.

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

For more information about `SQLiteOpenFlags`, see [SQLite open flags](#sqlite-open-flags).

## Create a database access class

A database wrapper class provides an abstraction to the rest of the application. This centralizes query logic and simplifies the management of database initialization, making it easier to refactor or expand data operations as the application grows.

The Todo app defines the following `TodoItemDatabase` class:

```csharp
public class TodoItemDatabase
{
    static readonly Lazy<SQLiteAsyncConnection> lazyInitializer = new Lazy<SQLiteAsyncConnection>(() =>
    {
        return new SQLiteAsyncConnection(Constants.DatabasePath, Constants.Flags);
    });

    static SQLiteAsyncConnection Database => lazyInitializer.Value;
    static bool initialized = false;

    public TodoItemDatabase()
    {
        InitializeAsync().SafeFireAndForget(false);
    }

    async Task InitializeAsync()
    {
        if (!initialized)
        {
            if (!Database.TableMappings.Any(m => m.MappedType.Name == typeof(TodoItem).Name))
            {
                await Database.CreateTablesAsync(CreateFlags.None, typeof(TodoItem)).ConfigureAwait(false);
                initialized = true;
            }
        }
    }

    public Task<List<TodoItem>> GetItemsAsync()
    {
        return Database.Table<TodoItem>().ToListAsync();
    }

    public Task<List<TodoItem>> GetItemsNotDoneAsync()
    {
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

### Lazy initialization

The `TodoItemDatabase` uses the .NET `Lazy` class to delay initialization of the database until it is first accessed. Using lazy initialization prevents the database loading process from delaying the app launch. For more information about the `Lazy` class, see [Lazy<T> Class](https://docs.microsoft.com//api/system.lazy-1).

The database connection is a static field. This ensures that a single database connection is used for the life of the app, which improves performance.

The `InitializeAsync` method is responsible for checking if a table already exists for storing `TodoItem` objects. This method will automatically create the table if it does not exist.

### SQLite open flags

The sample app database is initilized with three `SQLiteOpenFlag` enum values:

- `SQLiteOpenFlags.ReadWrite`: Allows the database connection to read and write data.
- `SQLiteOpenFlags.Create`: Allows the database file to be automatically created if it doesn't already exist.
- `SQLiteOpenFlags.SharedCache`: Allows multi-threaded access to the database.

You may need to specify different flags depending on how your database will be used. For more information about `SQLiteOpenFlags`, see [Opening A New Database Connection](https://www.sqlite.org/c3ref/open.html).

### The SafeFireAndForget extension method

When the `TodoItemDatabase` class is instantiated, it must initialize the database connection, which is an asynchronous process. However:

- Class constructors cannot be asynchronous.
- An async method that is not awaited will not throw exceptions.
- Using the `Wait` method blocks the thread _and_ swallows exceptions.

In order to start the asynchronous initialization, avoid blocking execution, and have the opportunity to catch exceptions, the sample application uses an extension method called `SafeFireAndForget`. The `SafeFireAndForget` extension method is defined on the `Task` class:

```csharp
public static class TaskExtensions
{
    // NOTE: Async void is intentional here. This provides a way
    // to call an async method from the constructor while
    // communicating intent to fire and forget, and allow
    // handling of exceptions
    public static async void SafeFireAndForget(this Task task,
        bool returnToCallingContext,
        Action<Exception> onException = null)
    {
        try
        {
            await task.ConfigureAwait(returnToCallingContext);
        }

        // if the provided action is not null, catch and
        // pass the thrown exception
        catch (Exception ex) when (onException != null)
        {
            onException(ex);
        }
    }
}
```

The `SafeFireAndForget` method awaits the asynchronous execution and allows developers to attach an `Action` that is called if an exception is thrown.

For more information, see [Task-based asynchronous pattern (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)

## Store and retrieve data in Xamarin.Forms

The `TodoItemDatabase` includes methods for the four types of data manipulation: create, read, edit and delete. The Xamarin.Forms `App` class exposes an instance of the `TodoItemDatabase` class:

```csharp
public static TodoItemDatabase Database
{
    get
    {
        if (database == null)
        {
            database = new TodoItemDatabase();
        }
        return database;
    }
}
```

This property allows Xamarin.Forms components to call data retrieval and manipulation methods on the `Database` instance in response to user interaction. For example:

```csharp
var saveButton = new Button { Text = "Save" };
saveButton.Clicked += async (sender, e) =>
{
    var todoItem = (TodoItem)BindingContext;
    await App.Database.SaveItemAsync(todoItem);
    await Navigation.PopAsync();
};
```

## Configure advanced features

### ConfigureAwait

### Copy or backup a database

### Use Polly

## Related Links

- Sample application
- SQLite Nuget package
- SQLite documentation
- Task-Asynchronous Pattern (TAP)
- Lazy initialization