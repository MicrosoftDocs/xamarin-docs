---
title: "Hello, Android Multiscreen: Deep Dive"
description: "In this two-part guide, the basic Phoneword application (created in the Hello, Android guide) is expanded to handle a second screen. Along the way, the basic Android Application Building Blocks are introduced. A deeper dive into Android architecture is included to help you develop a better understanding of Android application structure and functionality."
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: E4150036-7760-4023-BD33-B7BDE7B7AF5B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 10/05/2018
---

# Hello, Android Multiscreen: Deep Dive

_In this two-part guide, the basic Phoneword application (created in the Hello, Android guide) is expanded to handle a second screen. Along the way, the basic Android application building blocks are introduced. A deeper dive into Android architecture is included to help you develop a better understanding of Android application structure and functionality._

In the
[Hello, Android Multiscreen Quickstart](~/android/get-started/hello-android-multiscreen/hello-android-multiscreen-quickstart.md),
you built and ran your first multi-screen Xamarin.Android application.

In this guide you will explore more advanced Android architecture. Android
navigation with *Intents* is explained, and Android hardware navigation
options are explored. New additions to the Phoneword app are dissected
as you develop a more holistic view of the application's relationship
with the operating system and other applications.

## Android architecture basics

In the [Hello, Android Deep Dive](~/android/get-started/hello-android/hello-android-deepdive.md),
you learned that Android applications are unique programs because they
lack a single entry point. Instead, the operating system (or another
application) starts any one of the application's registered Activities,
which in turn starts the process for the application. This deep dive
into Android architecture expands your understanding of how Android
applications are constructed by introducing the Android Application
Building Blocks and their functions.

### Android application building blocks

An Android application consists of a collection of special Android
classes called *Application Blocks* bundled together with any number of
app resources - images, themes, helper classes, etc. &ndash; these are
coordinated by an XML file called the *Android Manifest*.

Application Blocks form the backbone of Android applications because
they allow you to do things you couldn't normally accomplish with a
regular class. The two most important ones are _Activities_ and _Services_:

- **Activity** &ndash; An Activity corresponds to a screen with a user
    interface, and it is conceptually similar to a web page in a web
    application. For example, in a newsfeed application, the login
    screen would be the first Activity, the scrollable list of news
    items would be another Activity, and the details page for each item
    would be a third. You can learn more about Activities in the
    [Activity Lifecycle](~/android/app-fundamentals/activity-lifecycle/index.md)
    guide.

- **Service** &ndash; Android Services support Activities by taking over
    long-running tasks and running them in the background. Services
    don't have a user interface and are used to handle tasks that
    aren't tied to screens &ndash; for example, playing a song in the
    background or uploading photos to a server. For more information about
    Services, see the
    [Creating Services](~/android/app-fundamentals/services/index.md) and
    [Android Services](~/android/app-fundamentals/services/index.md)
    guides.

An Android application may not use all types of Blocks, and often has
several Blocks of one type. For example, the Phoneword application from
the [Hello, Android Quickstart](~/android/get-started/hello-android/hello-android-quickstart.md)
was composed of just one Activity (screen) and some resource files. A simple
music player app might have several Activities and a Service for
playing music when the app is in the background.

### Intents

Another fundamental concept in Android applications is the *Intent*.
Android is designed around the *principle of least privilege* &ndash;
applications have access only to the Blocks they require to work, and
they have limited access to the Blocks that make up the operating
system or other applications. Similarly, Blocks are *loosely-coupled*
&ndash; they are designed to have little knowledge of and limited
access to other Blocks (even blocks that are part of the same
application).

To communicate, Application Blocks send asynchronous messages
called *Intents* back and forth. Intents contain information about the
receiving Block and sometimes some data. An Intent sent from one App
component triggers something to happen in another App component,
binding the two App components and allowing them to communicate. By
sending Intents back and forth, you can get Blocks to coordinate complex
actions such as launching the camera app to take and save, gathering
location information, or navigating from one screen to the next.

### AndroidManifest.XML

When you add a Block to the application, it is registered with a
special XML file called the **Android Manifest**. The Manifest keeps
track of all Application Blocks in an application, as well as version
requirements, permissions, and linked libraries &ndash; everything that
the operating system needs to know for your application to run. The
**Android Manifest** also works with Activities and Intents to control
what actions are appropriate for a given Activity. These advanced
features of the Android Manifest are covered in the
[Working with the Android Manifest](~/android/platform/android-manifest.md)
guide.

In the single-screen version of the Phoneword application, only one
Activity, one Intent, and the `AndroidManifest.xml` were used, alongside
additional resources like icons. In the multi-screen version of
Phoneword, an additional Activity was added; it was launched from the
first Activity using an Intent. The next section explores how
Intents help to create navigation in Android applications.

## Android navigation

Intents were used to navigate between screens. It's time to
dive into this code to see how Intents work and understand their role
in Android navigation.

### Launching a second activity with an intent

In the Phoneword application, an Intent was used to launch a second
screen (Activity). Start by creating an Intent, passing in the
current *Context* (`this`, referring to the current **Context**) and
the type of Application Block that you're looking for (`TranslationHistoryActivity`):

```csharp
Intent intent = new Intent(this, typeof(TranslationHistoryActivity));
```

The **Context** is an interface to global information about the
application environment &ndash; it lets newly-created objects know what's
going on with the application. If you think of an Intent as a message,
you are providing the name of the message recipient
(`TranslationHistoryActivity`) and the receiver's address (`Context`).

Android provides an option to attach simple data to an Intent (complex
data is handled differently). In the Phoneword example,
`PutStringArrayExtra` is used to attach a list of phone numbers to the
Intent and `StartActivity` is called on the recipient of the
Intent. The completed code looks like this:

```csharp
translationHistoryButton.Click += (sender, e) =>
{
    var intent = new Intent(this, typeof(TranslationHistoryActivity));
    intent.PutStringArrayListExtra("phone_numbers", _phoneNumbers);
    StartActivity(intent);
};
```

## Additional concepts introduced in phoneword

The Phoneword application introduced several concepts not covered in
this guide. These concepts include:

**String Resources** &ndash; In the Phoneword application, the
text of the `TranslationHistoryButton` was set to `"@string/translationHistory"`. The
`@string` syntax means that the string's value is stored in the
_string resources file_, **Strings.xml**. The following
value for the `translationHistory` string was added to **Strings.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="translationHistory">Call History</string>
</resources>
```

For more information on string resources and other Android
resources, refer to the
[Android Resources guide](~/android/app-fundamentals/resources-in-android/index.md).

**ListView and ArrayAdapter** &ndash; A _ListView_ is a UI component
that provides a simple way to present a scrolling list of rows. A
`ListView` instance requires an _Adapter_ to feed it with data
contained in row views. The following line of code was used to
populate the user interface of `TranslationHistoryActivity`:

```csharp
this.ListAdapter = new ArrayAdapter<string>(this, Android.Resource.Layout.SimpleListItem1, phoneNumbers);
```

ListViews and Adapters are beyond the scope of this document, but
they are covered in the very comprehensive
[ListViews and Adapters](~/android/user-interface/layouts/list-view/index.md) guide.
[Populating a ListView With Data](~/android/user-interface/layouts/list-view/populating.md)
deals specifically with using built-in `ListActivity` and
`ArrayAdapter` classes to create and populate a `ListView` without
defining a custom layout, as was done in the Phoneword example.

## Summary

Congratulations, you've completed your first multi-screen Android
application! This guide introduced *Android Application Building
Blocks* and *Intents* and used them to build a multi-screened Android
application. You now have the solid foundation you need to start
developing your own Xamarin.Android applications.

Next, you'll learn to build cross-platform applications with Xamarin
in the
[Building Cross-Platform Applications guides](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md).
