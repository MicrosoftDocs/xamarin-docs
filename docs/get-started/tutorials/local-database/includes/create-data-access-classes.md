In this exercise you will add data access classes to the **LocalDatabaseTutorial** project, which will be used to persist data about people to the database.

# [Visual Studio](#tab/vswin)

1. In **Solution Explorer**, in the **LocalDatabaseTutorial** project, add a new class named `Person` to the project. Then, in **Person.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Person
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Name { get; set; }
            public int Age { get; set; }
        }
    }
    ```

    This code defines a `Person` class that will store data about each person in the application. The `ID` property is marked with `PrimaryKey` and `AutoIncrement` attributes to ensure that each `Person` instance in the database will have a unique id provided by SQLite.NET.

1. In **Solution Explorer**, in the **LocalDatabaseTutorial** project, add a new class named `Database` to the project. Then, in **Database.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Database
        {
            readonly SQLiteAsyncConnection _database;

            public Database(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Person>().Wait();
            }

            public Task<List<Person>> GetPeopleAsync()
            {
                return _database.Table<Person>().ToListAsync();
            }

            public Task<int> SavePersonAsync(Person person)
            {
                return _database.InsertAsync(person);
            }
        }
    }
    ```

    This class contains code to create the database, read data from it, and write data to it. The code uses asynchronous SQLite.NET APIs that move database operations to background threads. In addition, the `Database` constructor takes the path of the database file as an argument. This path will be provided by the `App` class in the next exercise.

1. In **Solution Explorer**, in the **LocalDatabaseTutorial** project, expand **App.xaml** and double-click **App.xaml.cs** to open it. Then, in **App.xaml.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace LocalDatabaseTutorial
    {
        public partial class App : Application
        {
            static Database database;

            public static Database Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new Database(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "people.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    This code defines a `Database` property that creates a new `Database` instance as a singleton. A local file path and filename, which represents where to store the database, are passed as the argument to the `Database` class constructor.

    > [!IMPORTANT]
    > The advantage of exposing the database as a singleton is that a single database connection is created that's kept open while the application runs, therefore avoiding the expense of opening and closing the database file each time a database operation is performed.

    For more information about the singleton design pattern, see [Design Patterns: Singleton](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Design-Patterns-Singleton).    

1. Build the solution to ensure there are no errors.

# [Visual Studio for Mac](#tab/vsmac)

1. In **Solution Pad**, in the **LocalDatabaseTutorial** project, add a new class named `Person` to the project. Then, in **Person.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Person
        {
            [PrimaryKey, AutoIncrement]
            public int ID { get; set; }
            public string Name { get; set; }
            public int Age { get; set; }
        }
    }
    ```

    This code defines a `Person` class that will store data about each person in the application. The `ID` property is marked with `PrimaryKey` and `AutoIncrement` attributes to ensure that each `Person` instance in the database will have a unique id provided by SQLite.NET.

1. In **Solution Pad**, in the **LocalDatabaseTutorial** project, add a new class named `Database` to the project. Then, in **Database.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System.Collections.Generic;
    using System.Threading.Tasks;
    using SQLite;

    namespace LocalDatabaseTutorial
    {
        public class Database
        {
            readonly SQLiteAsyncConnection _database;

            public Database(string dbPath)
            {
                _database = new SQLiteAsyncConnection(dbPath);
                _database.CreateTableAsync<Person>().Wait();
            }

            public Task<List<Person>> GetPeopleAsync()
            {
                return _database.Table<Person>().ToListAsync();
            }

            public Task<int> SavePersonAsync(Person person)
            {
                return _database.InsertAsync(person);
            }
        }
    }
    ```

    This class contains code to create the database, read data from it, and write data to it. The code uses asynchronous SQLite.NET APIs that move database operations to background threads. In addition, the `Database` constructor takes the path of the database file as an argument. This path will be provided by the `App` class in the next exercise.

1. In **Solution Pad**, in the **LocalDatabaseTutorial** project, expand **App.xaml** and double-click **App.xaml.cs** to open it. Then, in **App.xaml.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using System.IO;
    using Xamarin.Forms;

    namespace LocalDatabaseTutorial
    {
        public partial class App : Application
        {
            static Database database;

            public static Database Database
            {
                get
                {
                    if (database == null)
                    {
                        database = new Database(Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "people.db3"));
                    }
                    return database;
                }
            }

            public App()
            {
                InitializeComponent();

                MainPage = new MainPage();
            }

            protected override void OnStart()
            {
                // Handle when your app starts
            }

            protected override void OnSleep()
            {
                // Handle when your app sleeps
            }

            protected override void OnResume()
            {
                // Handle when your app resumes
            }
        }
    }
    ```

    This code defines a `Database` property that creates a new `Database` instance as a singleton. A local file path and filename, which represents where to store the database, are passed as the argument to the `Database` class constructor.

    > [!IMPORTANT]
    > The advantage of exposing the database as a singleton is that a single database connection is created that's kept open while the application runs, therefore avoiding the expense of opening and closing the database file each time a database operation is performed.

    For more information about the singleton design pattern, see [Design Patterns: Singleton](https://channel9.msdn.com/Shows/Visual-Studio-Toolbox/Design-Patterns-Singleton).

1. Build the solution to ensure there are no errors.
