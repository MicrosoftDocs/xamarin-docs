---
title: "Xamarin.Forms MessagingCenter"
description: "This article explains how to use the Xamarin.Forms MessagingCenter to send and receive messages, to reduce coupling between classes such as view models."
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/01/2016
---

# Xamarin.Forms MessagingCenter

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/UsingMessagingCenter)

_Xamarin.Forms includes a simple messaging service to send and receive messages._

<a name="Overview" />

## Overview

Xamarin.Forms `MessagingCenter` enables view models and other components to communicate with without having to know anything about each other besides a simple Message contract.

<a name="How_the_MessagingCenter_Works" />

## How the MessagingCenter Works

There are two parts to `MessagingCenter`:

-  **Subscribe** - Listen for messages with a certain signature and perform some action when they are received. Multiple subscribers can be listening for the same message.
-  **Send** - Publish a message for listeners to act upon. If no listeners have subscribed then the message is ignored.


The `MessagingService` is a static class with `Subscribe` and `Send` methods that are used throughout the solution.

Messages have a string `message` parameter that is used as way to *address* messages. The `Subscribe` and `Send` methods use generic parameters to further control how messages are delivered - two messages with the same `message` text but different generic type arguments will not be delivered to the same subscriber.

The API for `MessagingCenter` is simple:

- `Subscribe<TSender> (object subscriber, string message, Action<TSender> callback, TSender source = null)`
- `Subscribe<TSender, TArgs> (object subscriber, string message, Action<TSender, TArgs> callback, TSender source = null)`
- `Send<TSender> (TSender sender, string message)`
- `Send<TSender, TArgs> (TSender sender, string message, TArgs args)`
- `Unsubscribe<TSender, TArgs> (object subscriber, string message)`
- `Unsubscribe<TSender> (object subscriber, string message)`

These methods are explained below.

<a name="Using_the_MessagingCenter" />

## Using the MessagingCenter

Messages may be sent as a result of user-interaction (like a button click), a system event (like controls changing state) or some other incident (like an asynchronous download completing). Subscribers might be listening to change the appearance of the user interface, save data or trigger some other operation.

### Simple String Message

The simplest message contains just a string in the `message` parameter. A `Subscribe` method that *listens* for a simple string message is shown below - notice the generic type specifying the sender is expected to be of type `MainPage`. Any classes in the solution can subscribe to the message using this syntax:

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) => {
    // do something whenever the "Hi" message is sent
});
```

In the `MainPage` class the following code *sends* the message. The `this` parameter is an instance of `MainPage`.

```csharp
MessagingCenter.Send<MainPage> (this, "Hi");
```

The string doesn't change - it indicates the *message type* and is used for determining which subscribers to notify. This sort of message is used to indicate that some event occurred, such as "upload completed", where no further information is required.

### Passing an Argument

To pass an argument with the message, specify the argument Type in the `Subscribe` generic arguments and in the Action signature.

```csharp
MessagingCenter.Subscribe<MainPage, string> (this, "Hi", (sender, arg) => {
    // do something whenever the "Hi" message is sent
    // using the 'arg' parameter which is a string
});
```

To send the message with argument, include the Type generic parameter and the value of the argument in the `Send` method call.

```csharp
MessagingCenter.Send<MainPage, string> (this, "Hi", "John");
```

This simple example uses a `string` argument but any C# object can be passed.

### Unsubscribe

An object can unsubscribe from a message signature so that no future messages are delivered. The `Unsubscribe` method syntax should reflect the signature of the message (so may need to include the generic Type parameter for the message argument).

```csharp
MessagingCenter.Unsubscribe<MainPage> (this, "Hi");
MessagingCenter.Unsubscribe<MainPage, string> (this, "Hi");
```

<a name="Summary" />

## Summary

The MessagingCenter is a simple way to reduce coupling, especially between view models. It can be used to send and receive simple messages or pass an argument between classes. Classes should unsubscribe from messages they no longer wish to receive.


## Related Links

- [MessagingCenterSample](https://developer.xamarin.com/samples/UsingMessagingCenter)
- [Xamarin.Forms Samples](https://github.com/xamarin/xamarin-forms-samples)
