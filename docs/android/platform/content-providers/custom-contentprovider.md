---
title: "Creating a Custom ContentProvider"
description: "The previous section demonstrated how to consume data from a built-in ContentProvider implementation. This section will explain how to build a custom ContentProvider and then consume its data."
ms.service: xamarin
ms.assetid: 36742B59-607E-070E-5D0E-B9C18917D3F4
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
---

# Creating a Custom ContentProvider

_The previous section demonstrated how to consume data from a built-in ContentProvider implementation. This section will explain how to build a custom ContentProvider and then consume its data._

## About ContentProviders

A content provider class must inherit from `ContentProvider`. It should
consist of an internal data store that is used to respond to queries
and it should expose Uris and MIME Types as constants to help consuming
code make valid requests for data.

### URI (Authority)

`ContentProviders` are accessed in Android using a Uri. An application
that exposes a `ContentProvider` sets the Uris that it will respond to
in its **AndroidManifest.xml** file. When the application is installed,
these Uris are registered so that other applications can access them.

In Mono for Android, the content provider class should have a
`[ContentProvider]` attribute to specify the Uri (or Uris) that should
be added to **AndroidManifest.xml**.

### Mime Type

The typical format for MIME Types consists of two parts. Android
`ContentProviders` commonly use these two strings for the first part of
the MIME Type:

1. `vnd.android.cursor.item` &ndash; to represent a single row, use the
   `ContentResolver.CursorItemBaseType` constant in code.

1. `vnd.android.cursor.dir` &ndash; for multiple rows, use the
   `ContentResolver.CursorDirBaseType` constant in code.

The second part of the MIME Type is specific to your application, and
should use a reverse-DNS standard with a `vnd.` prefix. The sample code
uses `vnd.com.xamarin.sample.Vegetables`.

### Data Model Metadata

Consuming applications need to construct Uri queries to access
different types of data. The base Uri can be expanded to refer to a
particular table of data and may also include parameters to filter the
results. The columns and clauses used with the resulting cursor to
display data must also be declared.

To ensure that only valid Uri queries are constructed, it is customary
to provide the valid strings as constant values. This makes it easier
to access the `ContentProvider` because it makes the values
discoverable via code-completion, and prevents typos in the strings.

In the previous example the `android.provider.ContactsContract` class
exposed the metadata for the Contacts data. For our custom
`ContentProvider` we will just expose the constants on the class
itself.

## Implementation

There are three steps to creating and consuming a custom `ContentProvider`:

1. **Create a database class** &ndash; Implement `SQLiteOpenHelper`.

2. **Create a `ContentProvider` class** &ndash; Implement
   `ContentProvider` with an instance of the database, metadata exposed
   as constant values and methods to access the data.

3. **Access the `ContentProvider` via its Uri** &ndash; Populate a
   `CursorAdapter` using the `ContentProvider`, accessed via its Uri.

As previously discussed, `ContentProviders` can be consumed from
applications other than where they are defined. In this example the
data is consumed in the same application, but keep in mind that other
applications can also access it as long as they know the Uri and
information about the schema (which is usually exposed as constant
values).

## Create a Database

Most `ContentProvider` implementations will be based on a `SQLite`
database. The example database code in
**SimpleContentProvider/VegetableDatabase.cs** creates a very simple
two-column database, as shown:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
  const string create_table_sql =
    "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
  const string DatabaseName = "vegetables.db";
  const int DatabaseVersion = 1;

  public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
  public override void OnCreate(SQLiteDatabase db)
  {
    db.ExecSQL(create_table_sql);
    // seed with data
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
    db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
  }
  public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
  {
    throw new NotImplementedException();
  }
}
```

The database implementation itself does not need any special
considerations to be exposed with a `ContentProvider`, however if you
intend to bind the `ContentProvider's` data to a `ListView` control
then a unique integer column named `_id` must be part of the result
set. See the
[ListViews and Adapters](~/android/user-interface/layouts/list-view/index.md)
document for more details on using the `ListView` control.

## Create the ContentProvider

The rest of this section gives step-by-step instructions on how the
**SimpleContentProvider/VegetableProvider.cs** example class was built.

### Initialize the Database

The first step is to subclass `ContentProvider` and add
the database that it will use.

```csharp
public class VegetableProvider : ContentProvider 
{
    VegetableDatabase vegeDB;
    public override bool OnCreate()
    {
       vegeDB = new VegetableDatabase(Context);
        return true;
    }
}
```

The rest of the code will form the actual content provider
implementation that allows the data to be discovered and queried.

## Add Metadata for Consumers

There are four different types of metadata that we are going to expose
on the `ContentProvider` class. Only the authority is required, the
rest are done by convention.

- **Authority** &ndash; The `ContentProvider` attribute *must* be added
  to the class so that it is registered with the Android when the
  application is installed.

- **Uri** &ndash; The `CONTENT_URI` is exposed as a constant so that it
  is easy to use in code. It should match the Authority, but include
  the scheme and base path.

- **MIME Types** &ndash; Lists of results and single results are
  treated as different content types, so we define two MIME Types to
  represent them.

- **InterfaceConsts** &ndash; Provide a constant value for each data
  column name, so that consuming code can easily discover and refer to
  them without risking typographical errors.

This code shows how each of these items is implemented, adding to the
database definition from the previous step:

```csharp
[ContentProvider(new string[] { CursorTableAdapter.VegetableProvider.AUTHORITY })]
public class VegetableProvider : ContentProvider 
{
   public const string AUTHORITY = "com.xamarin.sample.VegetableProvider";
   static string BASE_PATH = "vegetables";
   public static readonly Android.Net.Uri CONTENT_URI = Android.Net.Uri.Parse("content://" + AUTHORITY + "/" + BASE_PATH);
   // MIME types used for getting a list, or a single vegetable
   public const string VEGETABLES_MIME_TYPE = ContentResolver.CursorDirBaseType + "/vnd.com.xamarin.sample.Vegetables";
   public const string VEGETABLE_MIME_TYPE = ContentResolver.CursorItemBaseType + "/vnd.com.xamarin.sample.Vegetables";
   // Column names
   public static class InterfaceConsts {
       public const string Id = "_id";
       public const string Name = "name";
   }
   VegetableDatabase vegeDB;
   public override bool OnCreate()
   {
       vegeDB = new VegetableDatabase(Context);
       return true;
   }
}
```

## Implement the URI Parsing Helper

Because consuming code uses Uris to make requests of a
`ContentProvider`, we need to be able to parse those requests to
determine what data to return. The `UriMatcher` class can help to parse
Uris, once it has been initialized with the Uri patterns that the
`ContentProvider` supports.

The `UriMatcher` in the example will be initialized with two Uris:

1. *"com.xamarin.sample.VegetableProvider/vegetables"* &ndash; request
   to return the full list of vegetables.

2. *"com.xamarin.sample.VegetableProvider/vegetables/\#"* &ndash; where
   the \# is a placeholder for a numeric parameter (the `_id` of the
   row in the database). An asterisk placeholder ("\*") can also be
   used to match a text parameter.

In the code we use the constants to refer to metadata values like the
AUTHORITY and BASE\_PATH. The return codes will be used in methods that
do Uri parsing, to determine what data to return.

```csharp
const int GET_ALL = 0; // return code when list of Vegetables requested
const int GET_ONE = 1; // return code when a single Vegetable is requested by ID
static UriMatcher uriMatcher = BuildUriMatcher();
static UriMatcher BuildUriMatcher()
{
  var matcher = new UriMatcher(UriMatcher.NoMatch);
  // Uris to match, and the code to return when matched
  matcher.AddURI(AUTHORITY, BASE_PATH, GET_ALL); // all vegetables
  matcher.AddURI(AUTHORITY, BASE_PATH + "/#", GET_ONE); // specific vegetable by numeric ID
  return matcher;
}
```

This code is all private to the `ContentProvider` class. Refer to
[Google's UriMatcher documentation](xref:Android.Content.UriMatcher)
for further information.

## Implement the QueryMethod

The simplest `ContentProvider` method to implement is the `Query`
method. The implementation below uses the `UriMatcher` to parse the
`uri` parameter and call the correct database method. If the `uri`
contains an ID parameter then the integer is parsed out (using
`LastPathSegment`) and used in the database query.

```csharp
public override Android.Database.ICursor Query(Android.Net.Uri uri, string[] projection, string selection, string[] selectionArgs, string sortOrder)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return GetFromDatabase();
  case GET_ONE:
    var id = uri.LastPathSegment;
    return GetFromDatabase(id); // the ID is the last part of the Uri
  default:
    throw new Java.Lang.IllegalArgumentException("Unknown Uri: " + uri);
  }
}
Android.Database.ICursor GetFromDatabase()
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables", null);
}
Android.Database.ICursor GetFromDatabase(string id)
{
  return vegeDB.ReadableDatabase.RawQuery("SELECT _id, name FROM vegetables WHERE _id = " + id, null);
}
```

The `GetType` method must also be overridden. This method may be called
to determine the content type that will be returned for a given Uri.
This might tell the consuming application how to handle that data.

```csharp
public override String GetType(Android.Net.Uri uri)
{
  switch (uriMatcher.Match(uri)) {
  case GET_ALL:
    return VEGETABLES_MIME_TYPE; // list
  case GET_ONE:
    return VEGETABLE_MIME_TYPE; // single item
  default:
    throw new Java.Lang.IllegalArgumentExceptoin ("Unknown Uri: " + uri);
   }
}
```

## Implement the Other Overrides

Our simple example does not allow for editing or deletion of data, but
the Insert, Update and Delete methods must be implemented so add them
without an implementation:

```csharp
public override int Delete(Android.Net.Uri uri, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override Android.Net.Uri Insert(Android.Net.Uri uri, ContentValues values)
{
  throw new Java.Lang.UnsupportedOperationException();
}
public override int Update(Android.Net.Uri uri, ContentValues values, string selection, string[] selectionArgs)
{
  throw new Java.Lang.UnsupportedOperationException();
}
```

That completes the basic `ContentProvider` implementation. Once the
application has been installed, the data it exposes will be available
both inside the application but also to any other application that
knows the Uri to reference it.

## Access the ContentProvider

Once the `VegetableProvider` has been implemented, accessing it is done
the same way as the Contacts provider at the start of this document:
obtain a cursor using the specified Uri and then use an adapter to
access the data.

## Bind a ListView to a ContentProvider

To populate a `ListView` with data we use the Uri that corresponds to
the unfiltered list of vegetables. In the code we use the constant
value `VegetableProvider.CONTENT_URI`, which we know resolves to
`com.xamarin.sample.vegetableprovider/vegetables`. Our
`VegetableProvider.Query` implementation will return a cursor that can
then be bound to the `ListView`.

The code in `SimpleContentProvider/HomeScreen.cs` shows how simple it
is to display data from a `ContentProvider`:

```csharp
listView = FindViewById<ListView>(Resource.Id.List);
string[] projection = new string[] { VegetableProvider.InterfaceConsts.Id, VegetableProvider.InterfaceConsts.Name} ;
string[] fromColumns = new string[] { VegetableProvider.InterfaceConsts.Name };
int[] toControlIds = new int[] { Android.Resource.Id.Text1 };

// CursorLoader introduced in Honeycomb (3.0, API_11)
var loader = new CursorLoader(this,
   VegetableProvider.CONTENT_URI, projection, null, null, null);
cursor = (ICursor)loader.LoadInBackground();

// Create a SimpleCursorAdapter
adapter = new SimpleCursorAdapter(this, Android.Resource.Layout.SimpleListItem1, cursor, fromColumns, toControlIds);
listView.Adapter = adapter;
```

The resulting application looks like this:

[![Screenshot of app listing Vegetables, Fruits, Flower Buds, Legumes, Bulbs, Tubers](custom-contentprovider-images/api11-contentprovider2.png)](custom-contentprovider-images/api11-contentprovider2.png#lightbox)

## Retrieve a Single Item from a ContentProvider

A consuming application might also want to access single rows of data,
which can be done by constructing a different Uri that refers to a
specific row (for example).

Use `ContentResolver` directly to access a single item, by building up
a Uri with the required `Id`.

```csharp
Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
```

The complete method looks like this:

```csharp
protected void OnListItemClick(object sender, AdapterView.ItemClickEventArgs e)
{
  var id = e.Id;
  string[] projection = new string[] { "name" };
  var uri = Uri.WithAppendedPath(VegetableProvider.CONTENT_URI, id.ToString());
  ICursor vegeCursor = ContentResolver.Query(uri, projection, null, new string[] { id.ToString() }, null);
  string text = "";
  if (vegeCursor.MoveToFirst()) {
    text = vegeCursor.GetInt(0) + " " + vegeCursor.GetString(1);
    Android.Widget.Toast.MakeText(this, text, Android.Widget.ToastLength.Short).Show();
  }
  vegeCursor.Close();
}
```

## Related Links

- [SimpleContentProvider (sample)](/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)