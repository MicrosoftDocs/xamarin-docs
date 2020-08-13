---
title: "NSString in Xamarin.iOS and Xamarin.Mac"
description: "This document describes how Xamarin.iOS transparently converts NSString objects to C# string objects, when this does not happen."
ms.prod: xamarin
ms.assetid: 785744B3-42E2-4590-8F41-435325E609B9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
---

# NSString in Xamarin.iOS and Xamarin.Mac

The design of both Xamarin.iOS and Xamarin.Mac calls for the use API to expose of the native .NET string type, `string`, for string manipulation in C# and other .NET programming languages, and to expose string as the data type exposed by the API instead of the `NSString` data type.

This means that developers should not have to keep strings that are intended
to be used for calling into Xamarin.iOS & Xamarin.Mac API (Unified) in a special type
(`Foundation.NSString`), they can keep
using Mono's `System.String` for all of the operations, and whenever an API in
Xamarin.iOS or Xamarin.Mac requires a string, our API binding takes care of
marshaling the information.

For example, the Objective-C "text" property on a `UILabel` of type `NSString`,
is declared like this:

```objc
@property(nonatomic, copy) NSString *text
```

This is exposed in Xamarin.iOS as:

```csharp
class UILabel {
    public string Text { get; set; }
}
```

Behind the scenes, the implementation of this property marshals the C# string
into an `NSString` and calls the `objc_msgSend` method in the same way that
Objective-C would.

There are a handful of third-party Objective-C APIs that do not consume an
`NSString`, but instead consume a C string (a "*char*"). In those cases,
you can still use the C# string data type, but you must use the 
[[PlainString]](~/cross-platform/macios/binding/objective-c-libraries.md) 
attribute to inform the binding generator that this string
should not be marshaled as an `NSString`, but instead as a C string.

 <a name="Exceptions_to_the_Rule"></a>

## Exceptions to the Rule

In both Xamarin.iOS and Xamarin.Mac, we have made an exception to this rule. The decision between when we expose `string`s, and when we make an except and expose `NSString`s, is made if the `NSString` method could be doing a pointer comparison instead of a content comparison.

This could happen when an Objective-C APIs uses a public `NSString` constant as a token that represents some action, instead of comparing the actual contents of the string.

In those cases, `NSString` APIs are exposed, and there are a minority of APIs that have this. You will also notice that NSString properties are exposed in some classes. Those  `NSString` properties are exposed for items like notifications. Those are properties usually look like this:

```csharp
class Foo {
     public NSString FooNotification { get; }
}
```

Notifications are keys that are used for the `NSNotification` class when you want to register for a particular event being broadcast by the runtime.

Keys usually look something like this:

```csharp
class Foo {
     public NSString FooBarKey { get; }
}
```

Another place where `NSString`s are exposed in the API is as tokens that are
used as parameters to certain APIs in iOS or OS X that take `NSDictionary` objects
as parameters. The dictionary typically contains `NSString` keys. Xamarin.iOS, by
convention, names those static `NSString` properties by adding the “Key” name.
