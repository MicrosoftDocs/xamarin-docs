---
title: "Communicating Between Loosely Coupled Components"
ms.topic: article
ms.prod: xamarin
ms.assetid: 1194af33-8a91-48d2-88b5-b84d77f2ce69
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
---

# Communicating Between Loosely Coupled Components

The publish-subscribe pattern is a messaging pattern in which publishers send messages without having knowledge of any receivers, known as subscribers. Similarly, subscribers listen for specific messages, without having knowledge of any publishers.

Events in .NET implement the publish-subscribe pattern, and are the most simple and straightforward approach for a communication layer between components if loose coupling is not required, such as a control and the page that contains it. However, the publisher and subscriber lifetimes are coupled by object references to each other, and the subscriber type must have a reference to the publisher type. This can create memory management issues, especially when there are short lived objects that subscribe to an event of a static or long-lived object. If the event handler isn't removed, the subscriber will be kept alive by the reference to it in the publisher, and this will prevent or delay the garbage collection of the subscriber.

## Introduction to MessagingCenter

The Xamarin.Forms [`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) class implements the publish-subscribe pattern, allowing message-based communication between components that are inconvenient to link by object and type references. This mechanism allows publishers and subscribers to communicate without having a reference to each other, helping to reduce dependencies between components, while also allowing components to be independently developed and tested.

The [`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) class provides multicast publish-subscribe functionality. This means that there can be multiple publishers that publish a single message, and there can be multiple subscribers listening for the same message. Figure 4-1 illustrates this relationship:

![](communicating-between-loosely-coupled-components-images/messagingcenter.png "Multicast publish-subscribe functionality")

**Figure 4-1:** Multicast publish-subscribe functionality

Publishers send messages using the [`MessagingCenter.Send`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender%7D/p/TSender/System.String/) method, while subscribers listen for messages using the [`MessagingCenter.Subscribe`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) method. In addition, subscribers can also unsubscribe from message subscriptions, if required, with the [`MessagingCenter.Unsubscribe`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender%7D/p/System.Object/System.String/) method.

Internally, the [`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) class uses weak references. This means that it will not keep objects alive, and will allow them to be garbage collected. Therefore, it should only be necessary to unsubscribe from a message when a class no longer wishes to receive the message.

The eShopOnContainers mobile app uses the [`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) class to communicate between loosely coupled components. The app defines three messages:

-   The `AddProduct` message is published by the `CatalogViewModel` class when an item is added to the shopping basket. In return, the `BasketViewModel` class subscribes to the message and increments the number of items in the shopping basket in response. In addition, the `BasketViewModel` class also unsubscribes from this message.
-   The `Filter` message is published by the `CatalogViewModel` class when the user applies a brand or type filter to the items displayed from the catalogue. In return, the `CatalogView` class subscribes to the message and updates the UI so that only items that match the filter criteria are displayed.
-   The `ChangeTab` message is published by the `MainViewModel` class when the `CheckoutViewModel` navigates to the `MainViewModel` following the successful creation and submission of a new order. In return, the `MainView` class subscribes to the message and updates the UI so that the **My profile** tab is active, to show the user's orders.

> [!NOTE]
> **Note:** While the [`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) class permits communication between loosely-coupled classes, it does not offer the only architectural solution to this issue. For example, communication between a view model and a view can also be achieved by the binding engine and through property change notifications. In addition, communication between two view models can also be achieved by passing data during navigation.

In the eShopOnContainers mobile app,[`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) is used to update in the UI in response to an action occurring in another class. Therefore, messages are published on the UI thread, with subscribers receiving the message on the same thread.

>💡 **Tip:** Marshal to the UI thread when performing UI updates. If a message that's sent from a background thread is required to update the UI, process the message on the UI thread in the subscriber by invoking the [`Device.BeginInvokeOnMainThread`](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/) method.

For more information about [`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/), see [MessagingCenter](https://developer.xamarin.com/guides/xamarin-forms/application-fundamentals/messaging-center/) on the Xamarin Developer Center.

## Defining a Message

[`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) messages are strings that are used to identify messages. The following code example shows the messages defined within the eShopOnContainers mobile app:

```csharp
public class MessengerKeys  
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

Publishers notify subscribers of a message with one of the [`MessagingCenter.Send`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) overloads. The following code example demonstrates publishing the `AddProduct` message:

```csharp
MessagingCenter.Send(this, MessengerKeys.AddProduct, catalogItem);
```

In this example, the [`Send`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) method specifies three arguments:

-   The first argument specifies the sender class. The sender class must be specified by any subscribers who wish to receive the message.
-   The second argument specifies the message.
-   The third argument specifies the payload data to be sent to the subscriber. In this case the payload data is a `CatalogItem` instance.

The [`Send`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) method will publish the message, and its payload data, using a fire-and-forget approach. Therefore, the message is sent even if there are no subscribers registered to receive the message. In this situation, the sent message is ignored.

> [!NOTE]
> **Note:** The [`MessagingCenter.Send`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Send%7BTSender,TArgs%7D/p/TSender/System.String/TArgs/) method can use generic parameters to control how messages are delivered. Therefore, multiple messages that share a message identity but send different payload data types can be received by different subscribers.

## Subscribing to a Message

Subscribers can register to receive a message using one of the [`MessagingCenter.Subscribe`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) overloads. The following code example demonstrates how the eShopOnContainers mobile app subscribes to, and processes, the `AddProduct` message:

```csharp
MessagingCenter.Subscribe&lt;CatalogViewModel, CatalogItem&gt;(  
    this, MessageKeys.AddProduct, async (sender, arg) =&gt;  
{  
    BadgeCount++;  

    await AddCatalogItemAsync(arg);  
});
```

In this example, the [`Subscribe`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) method subscribes to the `AddProduct` message, and executes a callback delegate in response to receiving the message. This callback delegate, specified as a lambda expression, executes code that updates the UI.

>💡 **Tip:** Consider using immutable payload data. Don't attempt to modify the payload data from within a callback delegate because several threads could be accessing the received data simultaneously. In this scenario, the payload data should be immutable to avoid concurrency errors.

A subscriber might not need to handle every instance of a published message, and this can be controlled by the generic type arguments that are specified on the [`Subscribe`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Subscribe%7BTSender%7D/p/System.Object/System.String/System.Action%7BTSender%7D/TSender/) method. In this example, the subscriber will only receive `AddProduct` messages that are sent from the `CatalogViewModel` class, whose payload data is a `CatalogItem` instance.

## Unsubscribing from a Message

Subscribers can unsubscribe from messages they no longer want to receive. This is achieved with one of the [`MessagingCenter.Unsubscribe`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) overloads, as demonstrated in the following code example:

```csharp
MessagingCenter.Unsubscribe&lt;CatalogViewModel, CatalogItem&gt;(this, MessengerKeys.AddProduct);
```

In this example, the [`Unsubscribe`](https://developer.xamarin.com/api/member/Xamarin.Forms.MessagingCenter.Unsubscribe%7BTSender,TArgs%7D/p/System.Object/System.String/) method syntax reflects the type arguments specified when subscribing to receive the `AddProduct` message.

## Summary

The Xamarin.Forms [`MessagingCenter`](https://developer.xamarin.com/api/type/Xamarin.Forms.MessagingCenter/) class implements the publish-subscribe pattern, allowing message-based communication between components that are inconvenient to link by object and type references. This mechanism allows publishers and subscribers to communicate without having a reference to each other, helping to reduce dependencies between components, while also allowing components to be independently developed and tested.


## Related Links

- [Download eBook (2Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (sample)](https://github.com/dotnet-architecture/eShopOnContainers)
