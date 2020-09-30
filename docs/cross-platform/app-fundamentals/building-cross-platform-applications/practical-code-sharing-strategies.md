---
title: "Part 5 - Practical Code Sharing Strategies"
description: "This document discusses practical code sharing strategies for scenarios such as databases, file access, network operations, and asynchronous code."
ms.prod: xamarin
ms.assetid: 328D042A-FF78-A7B6-1574-B5AF49A1AADB
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
---

# Part 5 - Practical Code Sharing Strategies

This section gives examples of how to share code for common application
scenarios.

## Data Layer

The data layer consists of a storage engine and methods to read and write
information. For performance, flexibility and cross-platform compatibility the
SQLite database engine is recommended for Xamarin cross-platform applications.
It runs on a wide variety of platforms including Windows,
Android, iOS and Mac.

### SQLite

SQLite is an open-source database implementation. The source and
documentation can be found at [SQLite.org](https://www.sqlite.org/). SQLite support is available on each mobile
platform:

- **iOS** – Built in to the operating system.
- **Android** – Built in to the operating system since Android 2.2 (API Level 10).
- **Windows** – See the [SQLite for Universal Windows Platform extension](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936).

Even with the database engine available on all platforms, the native methods
to access the database are different. Both iOS and Android offer built-in APIs
to access SQLite that could be used from Xamarin.iOS or Xamarin.Android, however
using the native SDK methods offers no ability to share code (other than perhaps
the SQL queries themselves, assuming they’re stored as strings). For details
about native database functionality search for `CoreData` in iOS or
Android’s `SQLiteOpenHelper` class; because these options are not
cross-platform they are beyond the scope of this document.

### ADO.NET

Both Xamarin.iOS and Xamarin.Android support `System.Data` and `Mono.Data.Sqlite` (see the Xamarin.iOS [documentation](~/ios/data-cloud/system.data.md) for more info).
Using these namespaces allows you to write ADO.NET code that works on both
platforms. Edit the project’s references to include `System.Data.dll` and `Mono.Data.Sqlite.dll` and add these
using statements to your code:

```csharp
using System.Data;
using Mono.Data.Sqlite;
```

Then the following sample code will work:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "items.db3");
bool exists = File.Exists (dbPath);
if (!exists)
    SqliteConnection.CreateFile (dbPath);
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open ();
if (!exists) {
    // This is the first time the app has run and/or that we need the DB.
    // Copy a "template" DB from your assets, or programmatically create one like this:
    var commands = new[]{
        "CREATE TABLE [Items] (Key ntext, Value ntext);",
        "INSERT INTO [Items] ([Key], [Value]) VALUES ('sample', 'text')"
    };
    foreach (var command in commands) {
        using (var c = connection.CreateCommand ()) {
            c.CommandText = command;
            c.ExecuteNonQuery ();
        }
    }
}
// use `connection`... here, we'll just append the contents to a TextView
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT [Key], [Value] from [Items]";
    var r = contents.ExecuteReader ();
    while (r.Read ())
        Console.Write("\n\tKey={0}; Value={1}",
                r ["Key"].ToString (),
                r ["Value"].ToString ());
}
connection.Close ();
```

Real-world implementations of ADO.NET would obviously be split across
different methods and classes (this example is for demonstration purposes
only).

### SQLite-NET – Cross-Platform ORM

An ORM (or Object-Relational Mapper) attempts to simplify storage of data
modeled in classes. Rather than manually writing SQL queries that CREATE TABLEs
or SELECT, INSERT and DELETE data that is manually extracted from class fields
and properties, an ORM adds a layer of code that does that for you. Using
reflection to examine the structure of your classes, an ORM can automatically
create tables and columns that match a class and generate queries to read and
write the data. This allows application code to simply send and retrieve object
instances to the ORM, which takes care of all the SQL operations under the
hood.

SQLite-NET acts as a simple ORM that will allow you to save and retrieve your
classes in SQLite. It hides the complexity of cross platform SQLite access with
a combination of compiler directives and other tricks.

Features of SQLite-NET:

- Tables are defined by adding attributes to Model classes.
- A database instance is represented by a subclass of  `SQLiteConnection` , the main class in the SQLite-Net library.
- Data can be inserted, queried and deleted using objects. No SQL statements are required (although you can write SQL statements if required).
- Basic Linq queries can be performed on the collections returned by SQLite-NET.

The source code and documentation for SQLite-NET is available at [SQLite-Net on github](https://github.com/praeclarum/sqlite-net) and has been implemented in both case-studies. A simple example of
SQLite-NET code (from the *Tasky Pro* case study) is shown below.

First, the `TodoItem` class uses attributes to define a field to be a
database primary key:

```csharp
public class TodoItem : IBusinessEntity
{
    public TodoItem () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

This allows a `TodoItem` table to be created with the following line of code
(and no SQL statements) on an `SQLiteConnection` instance:

```csharp
CreateTable<TodoItem> ();
```

Data in the table can also be manipulated with other methods on the `SQLiteConnection`
(again, without requiring SQL statements):

```csharp
Insert (TodoItem); // 'task' is an instance with data populated in its properties
Update (TodoItem); // Primary Key field must be populated for Update to work
Table<TodoItem>.ToList(); // returns all rows in a collection
```

See the case study source code for complete examples.

## File Access

File access is certain to be a key part of any application. Common examples
of files that might be part of an application include:

- SQLite database files.
- User-generated data (text, images, sound, video).
- Downloaded data for caching (images, html or PDF files).

### System.IO Direct Access

Both Xamarin.iOS and Xamarin.Android allow file system access using classes in
the `System.IO` namespace.

Each platform does have different access restrictions that must be taken into
consideration:

- iOS applications run in a sandbox with very restricted file-system access. Apple further dictates how you should use the file system by specifying certain locations that are backed-up (and others that are not). Refer to the  [Working with the File System in Xamarin.iOS](~/ios/app-fundamentals/file-system.md) guide for more details.
- Android also restricts access to certain directories related to the application, but it also supports external media (eg. SD cards) and accessing shared data.
- Windows Phone 8 (Silverlight) do not allow direct file access – files can
  only be manipulated using `IsolatedStorage`.
- Windows 8.1 WinRT and Windows 10 UWP projects only offer asynchronous file
  operations via `Windows.Storage` APIs, which are different from the other platforms.

#### Example for iOS and Android

A trivial example that writes and reads a text file is shown below.
Using `Environment.GetFolderPath` allows the same code to run on iOS and
Android, which each return a valid directory based on their filesystem
conventions.

```csharp
string filePath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "MyFile.txt");
System.IO.File.WriteAllText (filePath, "Contents of text file");
Console.WriteLine (System.IO.File.ReadAllText (filePath));
```

Refer to the Xamarin.iOS [Working with the File System](~/ios/app-fundamentals/file-system.md) document for more information on iOS-specific filesystem
functionality. When writing cross-platform file access code, remember that some
file-systems are case-sensitive and have different directory separators. It is
good practice to always use the same casing for filenames and the `Path.Combine()` method when constructing file or directory
paths.

### Windows.Storage for Windows 8 and Windows 10

The *Creating Mobile Apps with Xamarin.Forms* [book](https://developer.xamarin.com/r/xamarin-forms/book/)
[Chapter 20. Async and File I/O](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf)
includes [samples for Windows 8.1 and Windows 10](https://github.com/xamarin/xamarin-forms-book-preview-2/tree/master/Chapter20).

Using a [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md) it's
possible to read and file files on these platforms using the supported APIs:

```csharp
StorageFolder localFolder = ApplicationData.Current.LocalFolder;
IStorageFile storageFile = await localFolder.CreateFileAsync("MyFile.txt",
                                        CreationCollisionOption.ReplaceExisting);
await FileIO.WriteTextAsync(storageFile, "Contents of text file");
```

Refer to the [book chapter](https://developer.xamarin.com/r/xamarin-forms/book/chapter20.pdf) for more details.

<a name="Isolated_Storage"></a>

### Isolated Storage on Windows Phone 7 & 8 (Silverlight)

Isolated Storage is a common API for saving and loading files across all
iOS, Android, and older Windows Phone platforms.

It is the default mechanism for file access in Windows Phone (Silverlight) that has been
implemented in Xamarin.iOS and Xamarin.Android to allow common file-access code
to be written. The `System.IO.IsolatedStorage` class can be
referenced across all three platforms in a [Shared Project](~/cross-platform/app-fundamentals/shared-projects.md).

Refer to the [Isolated Storage Overview for Windows Phone](/previous-versions/windows/apps/ff402541(v=vs.105)) for more information.

The Isolated Storage APIs are not available in [Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md). One alternative for PCL is the [PCLStorage NuGet](https://pclstorage.codeplex.com/)

### Cross-platform file access in PCLs

There is also a PCL-compatible NuGet – [PCLStorage](https://www.nuget.org/packages/PCLStorage/) –
that facilities cross-platform file access for Xamarin-supported platforms and
the latest Windows APIs.

## Network Operations

Most mobile applications will have networking component, for example:

- Downloading images, video and audio (eg. thumbnails, photos, music).
- Downloading documents (eg. HTML, PDF).
- Uploading user data (such as photos or text).
- Accessing web services or 3rd party APIs (including SOAP, XML or JSON).

The .NET Framework provides a few different classes for accessing network resources:
`HttpClient`, `WebClient`, and `HttpWebRequest`.

### HttpClient

The `HttpClient` class in the `System.Net.Http` namespace is available in
Xamarin.iOS, Xamarin.Android, and most Windows platforms. There is a
[Microsoft HTTP Client Library NuGet](https://www.nuget.org/packages/Microsoft.Net.Http/)
that can be used to bring this API into Portable Class Libraries
(and Windows Phone 8 Silverlight).

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get, "https://xamarin.com");
var response = await myClient.SendAsync(request);
```

### WebClient

The `WebClient` class provides a simple API to retrieve remote
data from remote servers.

Universal Windows Platform operations *must* be async, even though Xamarin.iOS and
Xamarin.Android support synchronous operations (which can be done on background
threads).

The code for a simple asychronous `WebClient` operation is:

```csharp
var webClient = new WebClient ();
webClient.DownloadStringCompleted += (sender, e) =>
{
    var resultString = e.Result;
    // do something with downloaded string, do UI interaction on main thread
};
webClient.Encoding = System.Text.Encoding.UTF8;
webClient.DownloadStringAsync (new Uri ("http://some-server.com/file.xml"));
```

 `WebClient` also has `DownloadFileCompleted` and `DownloadFileAsync` for retrieving binary data.

<a name="HttpWebRequest"></a>

### HttpWebRequest

`HttpWebRequest` offers more customization than `WebClient` and as a result requires more code to use.

The code for a simple synchronous `HttpWebRequest` operation
is:

```csharp
var request = HttpWebRequest.Create(@"http://some-server.com/file.xml ");
request.ContentType = "text/xml";
request.Method = "GET";
using (HttpWebResponse response = request.GetResponse() as HttpWebResponse)
{
    if (response.StatusCode != HttpStatusCode.OK)
        Console.WriteLine("Error fetching data. Server returned status code: {0}", response.StatusCode);
    using (StreamReader reader = new StreamReader(response.GetResponseStream()))
    {
        var content = reader.ReadToEnd();
        // do something with downloaded string, do UI interaction on main thread
    }
}
```

There is an example in our [Web Services documentation](~/cross-platform/data-cloud/web-services/index.md).

 <a name="Reachability"></a>

### Reachability

Mobile devices operate under a variety of network conditions from fast Wi-Fi
or 4G connections to poor reception areas and slow EDGE data links. Because of
this, it is good practice to detect whether the network is available and if so,
what type of network is available, before attempting to connect to remote
servers.

Actions a mobile app might take in these situations include:

- If the network is unavailable, advise the user. If they have manually disabled it (eg. Airplane mode or turning off Wi-Fi) then they can resolve the issue.
- If the connection is 3G, applications may behave differently (for example, Apple does not allow apps larger than 20Mb to be downloaded over 3G). Applications could use this information to warn the user about excessive download times when retrieving large files.
- Even if the network is available, it is good practice to verify connectivity with the target server before initiating other requests. This will prevent the app’s network operations from timing out repeatedly and also allow a more informative error message to be displayed to the user.

## WebServices

See our documentation on [Working with Web Services](~/cross-platform/data-cloud/web-services/index.md), which covers accessing REST, SOAP and WCF endpoints using
Xamarin.iOS. It is possible to hand-craft web service requests and parse the
responses, however there are libraries available to make this much simpler,
including Azure, RestSharp, and ServiceStack. Even basic WCF operations can be accessed
in Xamarin apps.

### Azure

Microsoft Azure is a cloud platform that provides
a wide variety of services for mobile apps, including data storage and sync, and
push notifications.

Visit [azure.microsoft.com](https://azure.microsoft.com/) to try
it for free.

### RestSharp

RestSharp is a .NET library that can be included in mobile applications to
provide a REST client that simplifies access to web services. It helps by
providing a simple API to request data and parse the REST response. RestSharp
can be useful

The [RestSharp website](http://restsharp.org/)
contains [documentation](https://github.com/restsharp/RestSharp/wiki) on how to implement a REST client using RestSharp.
RestSharp provides Xamarin.iOS and Xamarin.Android examples on [github](https://github.com/restsharp/RestSharp/).

There is also a Xamarin.iOS code snippet in our [Web Services documentation](~/cross-platform/data-cloud/web-services/index.md).

 <a name="ServiceStack"></a>

### ServiceStack

Unlike RestSharp, ServiceStack is both a server-side solution to host a web
service as well as a client library that can be implemented in mobile
applications to access those services.

The [ServiceStack website](http://servicestack.net/) explains the purpose of the project and links to document and code
samples. The examples include a complete server-side implementation of a web
service as well as various client-side applications that can access it.

### WCF

Xamarin tools can help you consume some Windows Communication Foundation
(WCF) services. In general, Xamarin supports the same
client-side subset of WCF that ships with the Silverlight runtime. This includes
the most common encoding and protocol implementations of WCF: text-encoded SOAP
messages over the HTTP transport protocol using the `BasicHttpBinding`.

Due to the size and complexity of the WCF framework, there may be current and
future service implementations that will fall outside of the scope supported by
Xamarin’s client-subset domain. In addition, WCF support requires the use of
tools only available in a Windows environment to generate the proxy.

 <a name="Threading"></a>

## Threading

Application responsiveness is important for mobile applications – users
expect applications to load and perform quickly. A ‘frozen’ screen that
stops accepting user-input will appear to indicate the application has crashed,
so it is important not to tie up the UI thread with long-running blocking calls
such as network requests or slow local operations (such as unzipping a file). In
particular the startup process should not contain long-running tasks – all
mobile platforms will kill an app that takes too long to load.

This means your user interface should implement a ‘progress indicator’ or
otherwise ‘useable’ UI that is quick to display, and asynchronous tasks to
perform background operations. Executing background tasks requires the use of
threads, which means the background tasks needs a way to communicate back to the
main thread to indicate progress or when they have completed.

 <a name="Parallel_Task_Library"></a>

### Parallel Task Library

Tasks created with the Parallel Task Library can run asynchronously and
return on their calling thread, making them very useful for triggering
long-running operations without blocking the user interface.

A simple parallel task operation might look like this:

```csharp
using System.Threading.Tasks;
void MainThreadMethod ()
{
    Task.Factory.StartNew (() => wc.DownloadString ("http://...")).ContinueWith (
        t => label.Text = t.Result, TaskScheduler.FromCurrentSynchronizationContext()
    );
}
```

The key is `TaskScheduler.FromCurrentSynchronizationContext()`
which will reuse the SynchronizationContext.Current of the thread calling the
method (here the main thread that is running `MainThreadMethod`) as a
way to marshal back calls to that thread. This means if the method is called on
the UI thread, it will run the `ContinueWith` operation back on the
UI thread.

If the code is starting tasks from other threads, use the following pattern
to create a reference to the UI thread and the task can still call back to
it:

```csharp
static Context uiContext = TaskScheduler.FromCurrentSynchronizationContext();
```

 <a name="Invoking_on_the_UI_Thread"></a>

### Invoking on the UI Thread

For code that doesn’t utilize the Parallel Task Library, each platform has
its own syntax for marshaling operations back to the UI thread:

- **iOS** – `owner.BeginInvokeOnMainThread(new NSAction(action))`
- **Android** – `owner.RunOnUiThread(action)`
- **Xamarin.Forms** – `Device.BeginInvokeOnMainThread(action)`
- **Windows** – `Deployment.Current.Dispatcher.BeginInvoke(action)`

Both the iOS and Android syntax requires a ‘context’ class to be
available which means the code needs to pass this object into any methods that
require a callback on the UI thread.

To make UI thread calls in shared code, follow the [IDispatchOnUIThread example](https://www.slideshare.net/follesoe/cross-platform-mobile-apps-using-net) (courtesy of [@follesoe](https://twitter.com/follesoe)). Declare and program
to an `IDispatchOnUIThread` interface in the shared code and then
implement the platform-specific classes as shown here:

```csharp
// program to the interface in shared code
public interface IDispatchOnUIThread {
    void Invoke (Action action);
}
// iOS
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly NSObject owner;
    public DispatchAdapter (NSObject owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.BeginInvokeOnMainThread(new NSAction(action));
    }
}
// Android
public class DispatchAdapter : IDispatchOnUIThread {
    public readonly Activity owner;
    public DispatchAdapter (Activity owner) {
        this.owner = owner;
    }
    public void Invoke (Action action) {
        owner.RunOnUiThread(action);
    }
}
// WP7
public class DispatchAdapter : IDispatchOnUIThread {
    public void Invoke (Action action) {
        Deployment.Current.Dispatcher.BeginInvoke(action);
    }
}
```

Xamarin.Forms developers should use [`Device.BeginInvokeOnMainThread`](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads)
in common code (Shared Projects or PCL).

 <a name="Platform_and_Device_Capabilities_and_Degradation"></a>

## Platform and Device Capabilities and Degradation

Further specific examples of dealing with different capabilities are given in
the Platform Capabilities documentation. It deals with detecting different
capabilities and how to gracefully degrade an application to provide a good user
experience, even when the app can’t operate to its full potential.