---
title: "Xamarin.Forms MessagingCenter"
description: "The Xamarin.Forms MessagingCenter class implements the publish-subscribe pattern, allowing message-based communication between components that are inconvenient to link by object and type references."
ms.prod: xamarin
ms.assetid: EDFE7B19-C5FD-40D5-816C-FAE56532E885
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/30/2019
---

# Xamarin.Forms MessagingCenter

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingmessagingcenter)

The publish-subscribe pattern is a messaging pattern in which publishers send messages without having knowledge of any receivers, known as subscribers. Similarly, subscribers listen for specific messages, without having knowledge of any publishers.

The Xamarin.Forms [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) class implements the publish-subscribe pattern, allowing message-based communication between components that are inconvenient to link by object and type references. This mechanism allows publishers and subscribers to communicate without having a reference to each other, helping to reduce dependencies between them.

The [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) class provides multicast publish-subscribe functionality. This means that there can be multiple publishers that publish a single message, and there can be multiple subscribers listening for the same message:

![](messaging-center-images/messaging-center.png "Multicast publish-subscribe functionality")

Publishers send messages using the [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) method, while subscribers listen for messages using the [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) method. In addition, subscribers can also unsubscribe from message subscriptions, if required, with the [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) method.

> [!IMPORTANT]
> Internally, the [`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) class uses weak references. This means that it will not keep objects alive, and will allow them to be garbage collected. Therefore, it should only be necessary to unsubscribe from a message when a class no longer wishes to receive the message.

## Publish a message

[`MessagingCenter`](xref:Xamarin.Forms.MessagingCenter) messages are strings that are used to identify messages. Publishers notify subscribers of a message with one of the [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) overloads. The following code example publishes a `Hi` message:

```csharp
MessagingCenter.Send<MainPage>(this, "Hi");
```

In this example, the [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) method specifies two arguments:

- The first argument specifies the sender class. The sender class must be specified by any subscribers who wish to receive the message.
- The second argument specifies the message.

Payload data can be sent with a message:

```csharp
MessagingCenter.Send<MainPage, string>(this, "Hi", "John");
```

In this example, the [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) method also specifies a third argument. The third argument specifies the payload data to be sent to the subscriber. In this case the payload data is a `string` instance.

In addition, the [`MessagingCenter.Send`](xref:Xamarin.Forms.MessagingCenter.Send*) method also specifies a generic argument. To receive the message, a subscriber must also specify the same generic argument. This enables multiple messages that share a message identity but send different payload data types to be received by different subscribers.

The [`Send`](xref:Xamarin.Forms.MessagingCenter.Send*) method will publish the message, and any payload data, using a fire-and-forget approach. Therefore, the message is sent even if there are no subscribers registered to receive the message. In this situation, the sent message is ignored.

## Subscribe to a message

Subscribers can register to receive a message using one of the [`MessagingCenter.Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) overloads. The following code example shows an example of this:

```csharp
MessagingCenter.Subscribe<MainPage> (this, "Hi", (sender) =>
{
    // Do something whenever the "Hi" message is sent
});
```

In this example, the [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) method subscribes to `Hi` messages sent by the `MainPage` type, and executes a callback delegate in response to receiving the message. This callback delegate, specified as a lambda expression, could be code that updates the UI.

> [!NOTE]
> A subscriber might not need to handle every instance of a published message, and this can be controlled by the generic type arguments that are specified on the [`Subscribe`](xref:Xamarin.Forms.MessagingCenter.Subscribe*) method.

In the following example, the subscriber will only receive `Hi` messages that are sent by the `MainPage` type, whose payload data is a `string`:

```csharp
MessagingCenter.Subscribe<MainPage, string>(this, "Hi", async (sender, arg) =>
{
    await DisplayAlert("Message received", "arg=" + arg, "OK");
});
```

## Unsubscribe from a message

Subscribers can unsubscribe from messages they no longer want to receive. This is achieved with one of the [`MessagingCenter.Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) overloads:

```csharp
MessagingCenter.Unsubscribe<MainPage>(this, "Hi");
```

In this example, the [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) method unsubscribes from the `Hi` message sent by the `MainPage` type.

Messages containing payload data can also be unsubscribed from:

```csharp
MessagingCenter.Unsubscribe<MainPage, string>(this, "Hi");
```

In this example, the [`Unsubscribe`](xref:Xamarin.Forms.MessagingCenter.Unsubscribe*) method unsubscribes from the `Hi` message sent by the `MainPage` type, whose payload data is a `string`.

## Related links

- [MessagingCenterSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/usingmessagingcenter)
- [Communicating Between Loosely Coupled Components](~/xamarin-forms/enterprise-application-patterns/communicating-between-loosely-coupled-components.md)
