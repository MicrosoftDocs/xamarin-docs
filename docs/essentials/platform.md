---
title: "Xamarin.Essentials Platform"
description: "The Platform class allows applications to run code on the main execution thread."
ms.assetid: CD6D51E7-D933-4FE7-A7F7-392EF27812E0
author: charlespetzold
ms.author: chape
ms.date: 06/03/2018
---

# Xamarin.Essentials Platform

![Pre-release NuGet](~/media/shared/pre-release.png)

The **Platform** class allows applications to run code on the main thread of execution, and to determine if a particular block of code is currently running on the main thread.

## Background

Most operating systems — including iOS, Android, and the Universal Windows Platform — use a single-threading model for code involving the user interface. This model is necessary to properly serialize user-interface events, including keystrokes and touch input. This thread is often called the _main thread_ or the _user-interface thread_ or the _UI thread_. The disadvantage of this model is that all code that accesses user interface elements must run on the application's main thread. 

Applications sometimes need to use events that call the event handler on a secondary thread of execution. (The Xamarin.Essentials classes [`Accelerometer`](accelerometer.md), [`Compass`](compass.md), [`Gyroscope`](gyroscope.md), [`Magnetometer`](magnetometer.md), and [`OrientationSensor`](orientation-sensor.md) all might return information on a secondary thread when used with faster speeds.) If the event handler needs to access user-interface elements, it must run that code on the main thread. The **Platform** class allows the application to run this code on the main thread.

## Running Code on the Main Thread

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

To run code on the main thread, call the static `Platform.BeginInvokeOnMainThread` method. The argument is an [`Action`](xref:System.Action) object, which is simply a method with no arguments and no return value:

```csharp
Platform.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

If you need to call this method in a Xamarin.Forms application within a class that derives from `Element` (which includes any class that derives from `View` or `Page`), the `Platform` class name needs to be fully qualified with the namespace name:

```csharp
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(() =>
{
    // Code to run on the main thread
});
```

It is also possible to define a separate method for the code that must run on the main thread:

```csharp
void MyMainThreadCode()
{
    // Code to run on the main thread
}
```

You can then run this method on the main thread by referencing it in the `BeginInvokeOnMainThread` method:

```csharp
Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
```

> [!NOTE]
> Xamarin.Forms has a method called
> [`Device.BeginInvokeOnMainThread(Action)`](https://docs.microsoft.com/dotnet/api/xamarin.forms.device.begininvokeonmainthread)
> that does the same thing as `Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)`. 
> While you can use either method in a Xamarin.Forms app, consider whether 
> or not the calling code has any other need for a dependency on 
> Xamarin.Forms. If not, `Xamarin.Essentials.Platform.BeginInvokeOnMainThread(Action)` 
> is likely a better option.

## Determining if Code is Running on the Main Thread

The `Platform` class also allows an application to determine if a particular block of code is running on the main thread. The `IsMainThread` property returns `true` if the code calling the property is running on the main thread. A program can use this property to run different code for the main thread or a secondary thread:

```csharp
if (Xamarin.Essentials.Platform.IsMainThread)
{
    // Code to run if this is the main thread
}
else
{
    // Code to run if this is a secondary thread
}
```

You might wonder if you should check if code is running on a secondary thread before calling `BeginInvokeOnMainThread`, for example, like this:

```csharp
if (Xamarin.Essentials.Platform.IsMainThread)
{
    MyMainThreadCode();
}
else
{
    Xamarin.Essentials.Platform.BeginInvokeOnMainThread(MyMainThreadCode);
}
```

You might suspect that this check might improve performance if the block of code is already running on the main thread.

_However, this check is not necessary._ The platform implementations of `BeginInvokeOnMainThread` themselves check if the call is made on the main thread. There is very little performance penalty if you call `BeginInvokeOnMainThread` when it's not really necessary.

## API

- [Platform source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Platform)
- [Platform API documentation](xref:Xamarin.Essentials.Platform)
