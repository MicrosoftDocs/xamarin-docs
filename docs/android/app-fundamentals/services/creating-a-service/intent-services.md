---
title: "Intent Services in Xamarin.Android"
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
---

# Intent Services in Xamarin.Android

Both started and bound services run on the main thread, which means that to keep performance smooth, a service needs to perform the work asynchronously. One of the simplest ways to address this concern is with a _worker queue processor pattern_, where the work to be done is placed in a queue that is serviced by a single thread.

The [`IntentService`](xref:Android.App.IntentService) is a subclass of the `Service` class that provides an Android specific implementation of this pattern. It will manage queueing work, starting up a worker thread to service the queue, and pulling requests off the queue to be run on the worker thread. An `IntentService` will quietly stop itself and remove the worker thread when there is no more work in the queue.

Work is submitted to the queue by creating an `Intent` and then passing that `Intent` to the `StartService` method.

It is not possible to stop or interrupt the `OnHandleIntent` method `IntentService` while it is working. Because of this design, an `IntentService` should be kept stateless &ndash; it should not rely on an active connection or communication from the rest of the application. An `IntentService` is meant to statelessly process the work requests.

There are two requirements for subclassing `IntentService`:

1. The new type (created by subclassing `IntentService`) only overrides the `OnHandleIntent` method.
2. The constructor for the new type requires a string which is used to name the worker thread that will handle the requests. The name of this worker thread is primarily used when debugging the application.

The following code shows an `IntentService` implementation with the overridden `OnHandleIntent` method:

```csharp
[Service]
public class DemoIntentService: IntentService
{
    public DemoIntentService () : base("DemoIntentService")
    {
    }

    protected override void OnHandleIntent (Android.Content.Intent intent)
    {
        Console.WriteLine ("perform some long running work");
        ...
        Console.WriteLine ("work complete");
    }
}
```

Work is sent to an `IntentService` by instantiating an `Intent` and then calling the [`StartService`](xref:Android.Content.Context.StartService*) method with that Intent as a parameter. The Intent will be passed to the service as a parameter in the `OnHandleIntent` method. This code snippet is an example of sending a work request to an Intent: 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.PutExtra("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

The `IntentService` can extract the values from the Intent, as demonstrated in this code snippet:  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");

    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```

## Related Links

- [IntentService](xref:Android.App.IntentService)
- [StartService](xref:Android.Content.Context.StartService*)
