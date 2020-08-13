---
title: "Part 2 - Architecture"
description: "This document describes architecture patterns helpful for building cross-platform applications. It discusses typical application layers (data layer, data access layer, etc.) and common mobile software patterns (MVVM, MVC, etc.)"
ms.prod: xamarin
ms.assetid: 2176DB2D-E84A-3757-CFAB-04A586068D50
author: davidortinau
ms.author: daortin
ms.date: 03/27/2017
---

# Part 2 - Architecture

A key tenet of building cross-platform apps is to create an architecture
that lends itself to a maximization of code sharing across platforms. Adhering
to the following Object Oriented Programming principles helps build a
well-architected application:

- **Encapsulation** – Ensuring that classes and even architectural layers only expose a minimal API that performs their required functions, and hides the implementation details. At a class level, this means that objects behave as ‘black boxes’ and that consuming code does not need to know how they accomplish their tasks. At an architectural level, it means implementing patterns like Façade that encourage a simplified API that orchestrates more complex interactions on behalf of the code in more abstract layers. This means that the UI code (for example) should only be responsible for displaying screens and accepting user-input; and never interacting with the database directly. Similarly the data-access code should only read and write to the database, but never interact directly with buttons or labels.
- **Separation of Responsibilities** – Ensure that each component (both at architectural and class level) has a clear and well-defined purpose. Each component should perform only its defined tasks and expose that functionality via an API that is accessible to the other classes that need to use it.
- **Polymorphism** – Programming to an interface (or abstract class) that supports multiple implementations means that core code can be written and shared across platforms, while still interacting with platform-specific features.

The natural outcome is an application modeled after real world or abstract
entities with separate logical layers. Separating code into layers make
applications easier to understand, test and maintain. It is recommended that the
code in each layer be physically separate (either in directories or even
separate projects for very large applications) as well as logically separate
(using namespaces).

 <a name="Typical_Application_Layers"></a>

## Typical Application Layers

Throughout this document and the case studies we refer to the following six
application layers:

- **Data Layer** – Non-volatile data persistence, likely to be an SQLite database but could be implemented with XML files or any other suitable mechanism.
- **Data Access Layer** – Wrapper around the Data Layer that provides Create, Read, Update, Delete (CRUD) access to the data without exposing implementation details to the caller. For example, the DAL may contain SQL statements to query or update the data but the referencing code would not need to know this.
- **Business Layer** – (sometimes called the Business Logic Layer or BLL) contains business entity definitions (the Model) and business logic. Candidate for Business Façade pattern.
- **Service Access Layer** – Used to access services in the cloud: from complex web services (REST, JSON, WCF) to simple retrieval of data and images from remote servers. Encapsulates the networking behavior and provides a simple API to be consumed by the Application and UI layers.
- **Application Layer** – Code that’s typically platform specific (not generally shared across platforms) or code that is specific to the application (not generally reusable). A good test of whether to place code in the Application Layer versus the UI Layer is (a) to determine whether the class has any actual display controls or (b) whether it could be shared between multiple screens or devices (eg. iPhone and iPad).
- **User Interface (UI) Layer** – The user-facing layer, contains screens, widgets and the controllers that manage them.

An application may not necessarily contain all layers – for example the
Service Access Layer would not exist in an application that does not access
network resources. A very simple application might merge the Data Layer and Data
Access Layer because the operations are extremely basic.

 <a name="Common_Mobile_Software_Patterns"></a>

## Common Mobile Software Patterns

Patterns are an established way to capture recurring solutions to common
problems. There are a few key patterns that are useful to understand in building
maintainable/understandable mobile applications.

- **Model, View, ViewModel (MVVM)** – The Model-View-ViewModel pattern is popular with frameworks that support data-binding, such as Xamarin.Forms. It was popularized by XAML-enabled SDKs like Windows Presentation Foundation (WPF) and Silverlight; where the ViewModel acts as a go-between between the data (Model) and user interface (View) via data binding and commands.
- **Model, View, Controller (MVC)** – A common and often misunderstood pattern, MVC is most often used when building User Interfaces and provides for a separation between the actual definition of a UI Screen (View), the engine behind it that handles interaction (Controller), and the data that populates it (Model). The model is actually a completely optional piece and therefore, the core of understanding this pattern lies in the View and Controller. MVC is a popular approach for iOS applications.
- **Business Façade** – AKA Manager Pattern, provides a simplified point of entry for complex work. For example, in a Task Tracking application, you might have a  `TaskManager` class with methods such as  `GetAllTasks()` ,  `GetTask(taskID)` ,  `SaveTask (task)` , etc. The  `TaskManager` class provides a Façade to the inner workings of actually saving/retrieving of tasks objects.
- **Singleton** – The Singleton pattern provides for a way in which only a single instance of a particular object can ever exist. For example, when using SQLite in mobile applications, you only ever want one instance of the database. Using the Singleton pattern is a simple way to ensure this.
- **Provider** – A pattern coined by Microsoft (arguably similar to Strategy, or basic Dependency Injection) to encourage code re-use across Silverlight, WPF and WinForms applications. Shared code can be written against an interface or abstract class, and platform-specific concrete implementations are written and passed in when the code is used.
- **Async** – Not to be confused with the Async keyword, the Async pattern is used when long-running work needs to be executed without holding up the UI or current processing. In its simplest form, the Async pattern simply describes that long-running tasks should be kicked off in another thread (or similar thread abstraction such as a Task) while the current thread continues to process and listens for a response from the background process, and then updates the UI when data and or state is returned.

Each of the patterns will be examined in more detail as their practical use
is illustrated in the case studies. Wikipedia has more detailed descriptions of
the [MVVM](https://en.wikipedia.org/wiki/Model–view–viewmodel),
[MVC](https://en.wikipedia.org/wiki/Model–view–controller),
[Facade](https://en.wikipedia.org/wiki/Facade_pattern), [Singleton](https://en.wikipedia.org/wiki/Singleton_pattern), [Strategy](https://en.wikipedia.org/wiki/Strategy_pattern) and [Provider](https://en.wikipedia.org/wiki/Provider_model) patterns (and of [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) generally).
