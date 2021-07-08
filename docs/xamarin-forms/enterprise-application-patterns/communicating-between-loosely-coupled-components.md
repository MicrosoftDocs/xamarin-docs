---
title: "Communicating Between Loosely Coupled Components"
description: "This chapter explains how the eShopOnContainers mobile app implements the publish-subscribe pattern, allowing message-based communication between components that are inconvenient to link by object and type references "
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Communicating Between Loosely Coupled Components

> [!NOTE]
> This eBook was published in the spring of 2017, and has not been updated since then. There is much in the book that remains valuable, but some of the material is outdated.

The publish-subscribe pattern is a messaging pattern in which publishers send messages without having knowledge of any receivers, known as subscribers. Similarly, subscribers listen for specific messages, without having knowledge of any publishers.

Events in .NET implement the publish-subscribe pattern, and are the most simple and straightforward approach for a communication layer between components if loose coupling is not required, such as a control and the page that contains it. However, the publisher and subscriber lifetimes are coupled by object references to each other, and the subscriber type must have a reference to the publisher type. This can create memory management issues, especially when there are short lived objects that subscribe to an event of a static or long-lived object. If the event handler isn't removed, the subscriber will be kept alive by the reference to it in the publisher, and this will prevent or delay the garbage collection of the subscriber.

## Introduction to MessagingCenter

The Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) class implements the publish-subscribe pattern, allowing message-based communication between components that are inconvenient to link by object and type references. This mechanism allows publishers and subscribers to communicate without having a reference to each other, helping to reduce dependencies between components, while also allowing components to be independently developed and tested.

The [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) class provides multicast publish-subscribe functionality. This means that there can be multiple publishers that publish a single message, and there can be multiple subscribers listening for the same message. Figure 4-1 illustrates this relationship:

![Multicast publish-subscribe functionality](communicating-between-loosely-coupled-components-images/messagingcenter.png)

**Figure 4-1:** Multicast publish-subscribe functionality

Publishers send messages using the [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) method, while subscribers listen for messages using the [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) method. In addition, subscribers can also unsubscribe from message subscriptions, if required, with the [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) method.

Internally, the [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) class uses weak references. This means that it will not keep objects alive, and will allow them to be garbage collected. Therefore, it should only be necessary to unsubscribe from a message when a class no longer wishes to receive the message.

The eShopOnContainers mobile app uses the [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) class to communicate between loosely coupled components. The app defines three messages:

- The `AddProduct` message is published by the `CatalogViewModel` class when an item is added to the shopping basket. In return, the `BasketViewModel` class subscribes to the message and increments the number of items in the shopping basket in response. In addition, the `BasketViewModel` class also unsubscribes from this message.
- The `Filter` message is published by the `CatalogViewModel` class when the user applies a brand or type filter to the items displayed from the catalogue. In return, the `CatalogView` class subscribes to the message and updates the UI so that only items that match the filter criteria are displayed.
- The `ChangeTab` message is published by the `MainViewModel` class when the `CheckoutViewModel` navigates to the `MainViewModel` following the successful creation and submission of a new order. In return, the `MainView` class subscribes to the message and updates the UI so that the **My profile** tab is active, to show the user's orders.

> [!NOTE]
> While the [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) class permits communication between loosely-coupled classes, it does not offer the only architectural solution to this issue. For example, communication between a view model and a view can also be achieved by the binding engine and through property change notifications. In addition, communication between two view models can also be achieved by passing data during navigation.

In the eShopOnContainers mobile app, [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) is used to update in the UI in response to an action occurring in another class. Therefore, messages are published on the UI thread, with subscribers receiving the message on the same thread.

> [!TIP]
> Marshal to the UI thread when performing UI updates. If a message that's sent from a background thread is required to update the UI, process the message on the UI thread in the subscriber by invoking the [`Device.BeginInvokeOnMainThread`](xref:Xamarin.Forms.Device.BeginInvokeOnMainThread(System.Action)) method.

For more information about [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter), see [MessagingCenter](~/xamarin-forms/app-fundamentals/messaging-center.md).

## Defining a Message

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) messages are strings that are used to identify messages. The following code example shows the messages defined within the eShopOnContainers mobile app:

```csharp
public class MessageKeys  
{  
    // Add product to basket  
    public const string AddProduct = "AddProduct";  

    // Filter  
    public const string Filter = "Filter";  

    // Change selected Tab programmatically  
    public const string ChangeTab = "ChangeTab";  
}
```

In this example, messages are defined using constants. The advantage of this approach is that it provides compile-time type safety and refactoring support.

## Publishing a Message

Publishers notify subscribers of a message with one of the [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) overloads. The following code example demonstrates publishing the `AddProduct` message:

```csharp
MessagingCenter.Send(this, MessageKeys.AddProduct, catalogItem);
```

In this example, the [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) method specifies three arguments:

- The first argument specifies the sender class. The sender class must be specified by any subscribers who wish to receive the message.
- The second argument specifies the message.
- The third argument specifies the payload data to be sent to the subscriber. In this case the payload data is a `CatalogItem` instance.

The [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) method will publish the message, and its payload data, using a fire-and-forget approach. Therefore, the message is sent even if there are no subscribers registered to receive the message. In this situation, the sent message is ignored.

> [!NOTE]
> The [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) method can use generic parameters to control how messages are delivered. Therefore, multiple messages that share a message identity but send different payload data types can be received by different subscribers.

## Subscribing to a Message

Subscribers can register to receive a message using one of the [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) overloads. The following code example demonstrates how the eShopOnContainers mobile app subscribes to, and processes, the `AddProduct` message:

```csharp
MessagingCenter.Subscribe<CatalogViewModel, CatalogItem>(  
    this, MessageKeys.AddProduct, async (sender, arg) =>  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

In this example, the [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) method subscribes to the `AddProduct` message, and executes a callback delegate in response to receiving the message. This callback delegate, specified as a lambda expression, executes code that updates the UI.

> [!TIP]
> Consider using immutable payload data. Don't attempt to modify the payload data from within a callback delegate because several threads could be accessing the received data simultaneously. In this scenario, the payload data should be immutable to avoid concurrency errors.

A subscriber might not need to handle every instance of a published message, and this can be controlled by the generic type arguments that are specified on the [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) method. In this example, the subscriber will only receive `AddProduct` messages that are sent from the `CatalogViewModel` class, whose payload data is a `CatalogItem` instance.

## Unsubscribing from a Message

Subscribers can unsubscribe from messages they no longer want to receive. This is achieved with one of the [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) overloads, as demonstrated in the following code example:

```csharp
MessagingCenter.Unsubscribe<CatalogViewModel, CatalogItem>(this, MessageKeys.AddProduct);
```

In this example, the [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) method syntax reflects the type arguments specified when subscribing to receive the `AddProduct` message.

## Summary

The Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) class implements the publish-subscribe pattern, allowing message-based communication between components that are inconvenient to link by object and type references. This mechanism allows publishers and subscribers to communicate without having a reference to each other, helping to reduce dependencies between components, while also allowing components to be independently developed and tested.

## Related Links

- [Download eBook (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
