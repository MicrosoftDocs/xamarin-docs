---
title: "Consume an Azure Cosmos DB Document Database in Xamarin.Forms"
description: "This article explains how to use the Azure Cosmos DB .NET Standard client library to integrate an Azure Cosmos DB document database into a Xamarin.Forms application."
ms.service: xamarin
ms.assetid: 7C0605D9-9B7F-4002-9B60-2B5DAA3EA30C
ms.subservice: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2017
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Consume an Azure Cosmos DB Document Database in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdb)

_An Azure Cosmos DB document database is a NoSQL database that provides low latency access to JSON documents, offering a fast, highly available, scalable database service for applications that require seamless scale and global replication. This article explains how to use the Azure Cosmos DB .NET Standard client library to integrate an Azure Cosmos DB document database into a Xamarin.Forms application._

> [!VIDEO https://youtube.com/embed/BoVH12igmbg]

**Microsoft Azure Cosmos DB video**

An Azure Cosmos DB document database account can be provisioned using an Azure subscription. Each database account can have zero or more databases. A document database in Azure Cosmos DB is a logical container for document collections and users.

An Azure Cosmos DB document database may contain zero or more document collections. Each document collection can have a different performance level, allowing more throughput to be specified for frequently accessed collections, and less throughput for infrequently accessed collections.

Each document collection consists of zero or more JSON documents. Documents in a collection are schema-free, and so do not need to share the same structure or fields. As documents are added to a document collection, Azure Cosmos DB automatically indexes them and they become available to be queried.

For development purposes, a document database can also be consumed through an emulator. Using the emulator, applications can be developed and tested locally, without creating an Azure subscription or incurring any costs. For more information about the emulator, see [Developing locally with the Azure Cosmos DB Emulator](/azure/cosmos-db/local-emulator/).

This article, and accompanying sample application, demonstrates a Todo list application where the tasks are stored in an Azure Cosmos DB document database. For more information about the sample application, see [Understanding the sample](~/xamarin-forms/data-cloud/web-services/introduction.md).

For more information about Azure Cosmos DB, see the [Azure Cosmos DB Documentation](/azure/cosmos-db/).

> [!NOTE]
> If you don't have an [Azure subscription](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), create a [free account](https://aka.ms/azfree-docs-mobileapps) before you begin.

## Setup

The process for integrating an Azure Cosmos DB document database into a Xamarin.Forms application is as follows:

1. Create an Azure Cosmos DB account. For more information, see [Create an Azure Cosmos DB account](/azure/cosmos-db/sql-api-dotnetcore-get-started#create-an-azure-cosmos-account).
1. Add the [Azure Cosmos DB .NET Standard client library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core) NuGet package to the platform projects in the Xamarin.Forms solution.
1. Add `using` directives for the `Microsoft.Azure.Documents`, `Microsoft.Azure.Documents.Client`, and `Microsoft.Azure.Documents.Linq` namespaces to classes that will access the Azure Cosmos DB account.

After performing these steps, the Azure Cosmos DB .NET Standard client library can be used to configure and execute requests against the document database.

> [!NOTE]
> The Azure Cosmos DB .NET Standard client library can only be installed into platform projects, and not into a Portable Class Library (PCL) project. Therefore, the sample application is a Shared Access Project (SAP) to avoid code duplication. However, the `DependencyService` class can be used in a PCL project to invoke Azure Cosmos DB .NET Standard client library code contained in platform-specific projects.

## Consuming the Azure Cosmos DB account

The `DocumentClient` type encapsulates the endpoint, credentials, and connection policy used to access the Azure Cosmos DB account, and is used to configure and execute requests against the account. The following code example demonstrates how to create an instance of this class:

```csharp
DocumentClient client = new DocumentClient(new Uri(Constants.EndpointUri), Constants.PrimaryKey);
```

The Azure Cosmos DB Uri and primary key must be provided to the `DocumentClient` constructor. These can be obtained from the Azure Portal. For more information, see [Connect to an Azure Cosmos DB account](/azure/cosmos-db/sql-api-dotnetcore-get-started#Connect).

### Creating a Database

A document database is a logical container for document collections and users, and can be created in the Azure Portal, or programmatically using the `DocumentClient.CreateDatabaseIfNotExistsAsync` method:

```csharp
public async Task CreateDatabase(string databaseName)
{
  ...
  await client.CreateDatabaseIfNotExistsAsync(new Database
  {
    Id = databaseName
  });
  ...
}
```

The `CreateDatabaseIfNotExistsAsync` method specifies a `Database` object as an argument, with the `Database` object specifying the database name as its `Id` property. The `CreateDatabaseIfNotExistsAsync` method creates the database if it doesn't exist, or returns the database if it already exists. However, the sample application ignores any data returned by the `CreateDatabaseIfNotExistsAsync` method.

> [!NOTE]
> The `CreateDatabaseIfNotExistsAsync` method returns a `Task<ResourceResponse<Database>>` object, and the status code of the response can be checked to determine whether a database was created, or an existing database was returned.

### Creating a Document Collection

A document collection is a container for JSON documents, and can be created in the Azure Portal, or programmatically using the `DocumentClient.CreateDocumentCollectionIfNotExistsAsync` method:

```csharp
public async Task CreateDocumentCollection(string databaseName, string collectionName)
{
  ...
  // Create collection with 400 RU/s
  await client.CreateDocumentCollectionIfNotExistsAsync(
    UriFactory.CreateDatabaseUri(databaseName),
    new DocumentCollection
    {
      Id = collectionName
    },
    new RequestOptions
    {
      OfferThroughput = 400
    });
  ...
}
```

The `CreateDocumentCollectionIfNotExistsAsync` method requires two compulsory arguments – a database name specified as a `Uri`, and a `DocumentCollection` object. The `DocumentCollection` object represents a document collection whose name is specified with the `Id` property. The `CreateDocumentCollectionIfNotExistsAsync` method creates the document collection if it doesn't exist, or returns the document collection if it already exists. However, the sample application ignores any data returned by the `CreateDocumentCollectionIfNotExistsAsync` method.

> [!NOTE]
> The `CreateDocumentCollectionIfNotExistsAsync` method returns a `Task<ResourceResponse<DocumentCollection>>` object, and the status code of the response can be checked to determine whether a document collection was created, or an existing document collection was returned.

Optionally, the `CreateDocumentCollectionIfNotExistsAsync` method can also specify a `RequestOptions` object, which encapsulates options that can be specified for requests issued to the Azure Cosmos DB account. The `RequestOptions.OfferThroughput` property is used to define the performance level of the document collection, and in the sample application, is set to 400 request units per second. This value should be increased or decreased depending on whether the collection will be frequently or infrequently accessed.

> [!IMPORTANT]
> Note that the `CreateDocumentCollectionIfNotExistsAsync` method will create a new collection with a reserved throughput, which has pricing implications.

### Retrieving Document Collection Documents

The contents of a document collection can be retrieved by creating and executing a document query. A document query is created with the `DocumentClient.CreateDocumentQuery` method:

```csharp
public async Task<List<TodoItem>> GetTodoItemsAsync()
{
  ...
  var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
            .AsDocumentQuery();
  while (query.HasMoreResults)
  {
    Items.AddRange(await query.ExecuteNextAsync<TodoItem>());
  }
  ...
}
```

This query asynchronously retrieves all the documents from the specified collection, and places the documents in a `List<TodoItem>` collection for display.

The `CreateDocumentQuery<T>` method specifies a `Uri` argument that represents the collection that should be queried for documents. In this example, the `collectionLink` variable is a class-level field that specifies the `Uri` that represents the document collection to retrieve documents from:

```csharp
Uri collectionLink = UriFactory.CreateDocumentCollectionUri(Constants.DatabaseName, Constants.CollectionName);
```

The `CreateDocumentQuery<T>` method creates a query that is executed synchronously, and returns an `IQueryable<T>` object. However, the `AsDocumentQuery` method converts the `IQueryable<T>` object to an `IDocumentQuery<T>` object which can be executed asynchronously. The asynchronous query is executed with the `IDocumentQuery<T>.ExecuteNextAsync` method, which retrieves the next page of results from the document database, with the `IDocumentQuery<T>.HasMoreResults` property indicating whether there are additional results to be returned from the query.

Documents can be filtered server side by including a `Where` clause in the query, which applies a filtering predicate to the query against the document collection:

```csharp
var query = client.CreateDocumentQuery<TodoItem>(collectionLink)
          .Where(f => f.Done != true)
          .AsDocumentQuery();
```

This query retrieves all documents from the collection whose `Done` property is equal to `false`.

### Inserting a Document into a Document Collection

Documents are user defined JSON content, and can be inserted into a document collection with the `DocumentClient.CreateDocumentAsync` method:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.CreateDocumentAsync(collectionLink, item);
  ...
}
```

The `CreateDocumentAsync` method specifies a `Uri` argument that represents the collection the document should be inserted into, and an `object` argument that represents the document to be inserted.

### Replacing a Document in a Document Collection

Documents can be replaced in a document collection with the `DocumentClient.ReplaceDocumentAsync` method:

```csharp
public async Task SaveTodoItemAsync(TodoItem item, bool isNewItem = false)
{
  ...
  await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, item.Id), item);
  ...
}
```

The `ReplaceDocumentAsync` method specifies a `Uri` argument that represents the document in the collection that should be replaced, and an `object` argument that represents the updated document data.

### Deleting a Document from a Document Collection

A document can be deleted from a document collection with the `DocumentClient.DeleteDocumentAsync` method:

```csharp
public async Task DeleteTodoItemAsync(string id)
{
  ...
  await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(Constants.DatabaseName, Constants.CollectionName, id));
  ...
}
```

The `DeleteDocumentAsync` method specifies a `Uri` argument that represents the document in the collection that should be deleted.

### Deleting a Document Collection

A document collection can be deleted from a database with the `DocumentClient.DeleteDocumentCollectionAsync` method:

```csharp
await client.DeleteDocumentCollectionAsync(collectionLink);
```

The `DeleteDocumentCollectionAsync` method specifies a `Uri` argument that represents the document collection to be deleted. Note that invoking this method will also delete the documents stored in the collection.

### Deleting a Database

A database can be deleted from an Azure Cosmos DB database account with the `DocumentClient.DeleteDatabaesAsync` method:

```csharp
await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri(Constants.DatabaseName));
```

The `DeleteDatabaseAsync` method specifies a `Uri` argument that represents the database to be deleted. Note that invoking this method will also delete the document collections stored in the database, and the documents stored in the document collections.

## Summary

This article explained how to use the Azure Cosmos DB .NET Standard client library to integrate an Azure Cosmos DB document database into a Xamarin.Forms application. An Azure Cosmos DB document database is a NoSQL database that provides low latency access to JSON documents, offering a fast, highly available, scalable database service for applications that require seamless scale and global replication.

## Related Links

- [Todo Azure Cosmos DB (sample)](/samples/xamarin/xamarin-forms-samples/webservices-tododocumentdb)
- [Azure Cosmos DB Documentation](/azure/cosmos-db/)
- [Azure Cosmos DB .NET Standard client library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core)
- [Azure Cosmos DB API](/dotnet/api/overview/azure/cosmosdb/client?view=azure-dotnet&preserve-view=true)
