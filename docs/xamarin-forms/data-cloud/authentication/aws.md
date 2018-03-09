---
title: "Authenticating Users with an Amazon SimpleDB Service"
description: "Amazon SimpleDB does not offer its own resource-based permissions system. Instead, authentication against an identity provider can be used to ensure that users only have access to their own data in the SimpleDB domain. This article explains how to restrict users' access to their own SimpleDB data."
ms.topic: article
ms.prod: xamarin
ms.assetid: 797C91A5-9720-4DAC-89D8-5C85996584C8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/20/2016
---

# Authenticating Users with an Amazon SimpleDB Service

_Amazon SimpleDB does not offer its own resource-based permissions system. Instead, authentication against an identity provider can be used to ensure that users only have access to their own data in the SimpleDB domain. This article explains how to restrict users' access to their own SimpleDB data._

[Xamarin.Auth](https://github.com/xamarin/Xamarin.Auth) is used in the sample application to manage the user authentication process and securely store users' account details on the device. For more information see [Authenticating Users with an Identity Provider](~/xamarin-forms/data-cloud/authentication/oauth.md).

## Allowing an Authenticated User Access to SimpleDB Domain Data

The sample application uses the `TodoItem` class to model data. To store a `TodoItem` instance in a SimpleDB service it must first be converted into a `List` of `ReplaceableAttribute` objects. For more information see [Creating SimpleDB objects](~/xamarin-forms/data-cloud/consuming/aws.md).

> [!NOTE]
> In iOS 9 and greater, App Transport Security (ATS) enforces secure connections between internet resources (such as the app's back-end server) and the app, thereby preventing accidental disclosure of sensitive information. Since ATS is enabled by default in apps built for iOS 9, all connections will be subject to ATS security requirements. If connections do not meet these requirements, they will fail with an exception.
> ATS can be opted out of if it is not possible to use the `HTTPS` protocol and secure communication for internet resources. This can be achieved by updating the app's **Info.plist** file. For more information see [App Transport Security](~/ios/app-fundamentals/ats.md).

To ensure that users have access to only their own data in the SimpleDB domain, the `ToSimpleDBReplaceableAttributes` method stores an additional attribute for a `TodoItem` instance, as shown in the following code example:

```csharp
List<ReplaceableAttribute> ToSimpleDBReplaceableAttributes (TodoItem item)
{
  return new List<ReplaceableAttribute> () {
    ...
    new ReplaceableAttribute () {
      Name = "User",
      Value = App.User.Email,
      Replace = true
    },
  };
}
```

This attribute ensures that each item stored in the SimpleDB domain has an associated email address for a user, which is used to uniquely identify the user the data belongs to. When the contents of a domain are retrieved by calling the `AmazonSimpleDBClient.SelectAsync` method, the query expression ensures that only items for the authenticated user are retrieved, as shown in the following code example:

```csharp
public async Task<List<TodoItem>> RefreshDataAsync ()
{
  ...
  var request = new SelectRequest () {
    SelectExpression = string.Format ("SELECT * from {0} WHERE User = '{1}'", tableName, App.User.Email)
  };
  var response = await client.SelectAsync (request);
  ...
}
```

The `SelectAsync` method returns a response containing a collection of items and associated attributes that match the query expression. The query expression ensures that only items that match the user's email address will be retrieved. For more information about query expressions, see [Using Select to Create Amazon SimpleDB Queries](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/UsingSelect.html) on Amazon's website.

> [!NOTE]
> Be careful to follow the quoting rules when constructing the query expression. For more information, see [Select Quoting Rules](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/QuotingRulesSelect.html) on Amazon's website.

## Summary

This article explained how to restrict users' access to their own SimpleDB data. Amazon SimpleDB does not offer its own resource-based permissions system. Instead, authentication against an identity provider can be used to ensure that users have access to only their own data in the SimpleDB domain.


## Related Links

- [TodoAWSAuth (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAWSAuth/)
- [Consuming an Amazon SimpleDB Service](~/xamarin-forms/data-cloud/consuming/aws.md)
- [Authenticating Users with an Identity Provider](~/xamarin-forms/data-cloud/authentication/oauth.md)
- [Amazon Cognito Identity](http://docs.aws.amazon.com/cognito/devguide/identity/)
- [Amazon SimpleDB Developer Documentation](http://docs.aws.amazon.com/AmazonSimpleDB/latest/DeveloperGuide/Welcome.html)
- [Amazon Web Services SDK for .NET](https://www.nuget.org/packages?q=Tags%3A%22aws-sdk-v3%22)
