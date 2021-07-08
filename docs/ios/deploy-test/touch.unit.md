---
title: "Unit Testing Xamarin.iOS Apps"
description: "This document provides an overview of how to unit test a Xamarin.iOS application. It describes how to create a unit test project, writing tests, and running tests."
ms.prod: xamarin
ms.assetid: BD959779-3239-79B6-5289-3A9ECDFBD973
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
---

# Unit Testing Xamarin.iOS Apps

This document describes how to create unit tests for your Xamarin.iOS projects.
Unit testing with Xamarin.iOS is done using the Touch.Unit framework that includes
both an iOS test runner as well as a modified version of NUnit called [Touch.Unit](https://github.com/xamarin/Touch.Unit) that
provides a familiar set of APIs for writing unit tests.

## Setting up a test project in Visual Studio for Mac

To setup a unit testing framework for your project, all you need to do is to
add to your solution a project of type **iOS Unit Tests Project**. Do this
by right-clicking on your solution and selecting **Add > Add New Project**. From the
list select **iOS > Tests > Unified API > iOS Unit Tests Project**
(you can choose either C# or F#).

![Choose either C# or F#](touch.unit-images/00.png)

The above will create a basic project that contains a basic runner program
and which references the new MonoTouch.NUnitLite assembly, your project will
look like this:

![The project in the Solution Explorer](touch.unit-images/01.png)

The `AppDelegate.cs` class contains the test runner, and it looks like
this:

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    UIWindow window;
    TouchRunner runner;

    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        window = new UIWindow (UIScreen.MainScreen.Bounds);
        runner = new TouchRunner (window);

        // register every tests included in the main application/assembly
        runner.Add (System.Reflection.Assembly.GetExecutingAssembly ());

        window.RootViewController = new UINavigationController (runner.GetViewController ());

        // make the window visible
        window.MakeKeyAndVisible ();

        return true;
    }
}
```

> [!NOTE]
> The iOS Unit Test project type is not available in Visual Studio 2019 or Visual Studio 2017 on Windows.

## Writing Some Tests

Now that you have the basic shell in place, you should write your first set
of tests.

Tests are written by creating classes that have the `[TestFixture]` attribute
applied to them. Inside each TestFixture class you should apply the `[Test]`
attribute to every method that you want the test runner to invoke. The actual
test fixtures can live in any file in your Tests project.

To quickly get started select **Add/Add New File** and select in the Xamarin.iOS
group **UnitTests**. This will add a skeleton file that contains one passing test,
one failing test and one ignored tests, it looks like this:

```csharp
using System;
using NUnit.Framework;

namespace Fixtures {

    [TestFixture]
    public class Tests {

        [Test]
        public void Pass ()
        {
                Assert.True (true);
        }

        [Test]
        public void Fail ()
        {
                Assert.False (true);
        }

        [Test]
        [Ignore ("another time")]
        public void Ignore ()
        {
                Assert.True (false);
        }
    }
}
```

## Running Your Tests

To run this project inside your solution right click on it and select **Debug Item**
or **Run Item**.

The test runner allows you to see which tests are registered and select
individually which tests can be executed.

[![The list of registered tests](touch.unit-images/02-sml.png)](touch.unit-images/02.png#lightbox) 
[![An individual text](touch.unit-images/03-sml.png)](touch.unit-images/03.png#lightbox) 

[![The run results](touch.unit-images/04-sml.png)](touch.unit-images/04.png#lightbox)

You can run individual test fixtures by selecting the text fixture from the
nested views, or you can run all of your tests with "Run Everything". If you run
the default test that is supposed to include one passing test, one failure and
one ignored test. This is what the report looks like, and you can drill down
directly to the failing tests and find out more information about the
failure:

[![Screenshot shows a sample report, Test Runner screen.](touch.unit-images/05-sml.png)](touch.unit-images/05.png#lightbox) [![Screenshot shows a sample report, Tests screen.](touch.unit-images/06-sml.png)](touch.unit-images/06.png#lightbox) [![Screenshot shows a sample report, Test screen with test status.](touch.unit-images/07-sml.png)](touch.unit-images/07.png#lightbox)

You can also look at the Application Output window in your IDE to see
which tests are being executed and their current status.

## Writing New Tests

NUnitLite is a modified version of the NUnit called [Touch.Unit](https://github.com/xamarin/Touch.Unit)
project. It is a lightweight testing framework for .NET, based on the
ideas in [NUnit](https://nunit.com/) and providing a subset of its features.
It uses minimal resources and will run on resource-restricted platforms such as
those used in embedded and mobile development. The NUnitLite API is available to you in
Xamarin.iOS. With the basic skeleton provided by the unit test template, your
main entry point are the [Assert class](xref:NUnit.Framework.Assert) methods.

In addition to the assert class methods, the unit testing functionality is
split on the following namespaces that are part of NUnitLite:

- [NUnit.Framework](xref:NUnit.Framework)
- [NUnit.Constraints](xref:NUnit.Framework.Constraints)
- [NUniteLite.Runner](xref:NUnitLite.Runner)
