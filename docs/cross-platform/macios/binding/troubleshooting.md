---
title: "Binding troubleshooting"
description: "This guide describes what to do if you have difficulty binding an Objective-C library. In particular, it discusses missing bindings, argument exceptions when passing null to a binding, and reporting bugs."
ms.prod: xamarin
ms.assetid: 7C65A55C-71FA-46C5-A1B4-955B82559844
author: davidortinau
ms.author: daortin
ms.date: 10/19/2016
---

# Binding troubleshooting

Some tips for troubleshooting bindings to macOS (formerly known as OS X) APIs in Xamarin.Mac.

## Missing bindings

While Xamarin.Mac covers much of the Apple APIs, sometimes you may need to call some Apple API that doesn’t have a binding yet. In other cases, you need to call third party C/Objective-C that it outside the scope of the Xamarin.Mac bindings.

If you are dealing with an Apple API, the first step is to let Xamarin know that you are hitting a section of the API that we don’t have coverage for yet. [File a bug](#reporting-bugs) noting the missing API. We use reports from customers to prioritize which APIs we work on next. In addition, if you have a Business or Enterprise license and this lack of a binding is blocking your progress, also follow the instructions at [Support](https://visualstudio.microsoft.com/vs/support/) to file a ticket. We can’t promise a binding, but in some cases we can get you a work around.

Once you notify Xamarin (if applicable) of your missing binding, the next step is to consider binding it yourself. We have a full guide [here](~/cross-platform/macios/binding/overview.md) and some unofficial documentation [here](https://brendanzagaeski.appspot.com/xamarin/0002.html) for wrapping Objective-C bindings by hand. If you are calling a C API, you can use C#’s P/Invoke mechanism, documentation is [here](https://www.mono-project.com/docs/advanced/pinvoke/).

If you decide to work on the binding yourself, be aware that mistakes in the binding can produce all sorts of interesting crashes in the native runtime. In particular, be very careful that your signature in C# matches the native signature in number of arguments and the size of each argument. Failure to do so may corrupt memory and/or the stack and you could crash immediately or at some arbitrary point in the future or corrupt data.

## Argument exceptions when passing null to a binding

While Xamarin works to provide high quality and well tested bindings for the Apple APIs, sometimes mistakes and bugs slip in. By far the most common issue that you might run into is an API throwing `ArgumentNullException` when you pass in null when the underlying API accepts `nil`. The native header files defining the API often do not provide enough information on which APIs accept nil and which will crash if you pass it in.

If you run into a case where passing in `null` throws an `ArgumentNullException` but you think it should work, follow these steps:

1. Check the Apple documentation and/or examples to see if you can find proof that it accepts `nil`. If you are comfortable with Objective-C, you can write a small test program to verify it.
2. [File a bug](#reporting-bugs).
3. Can you work around the bug? If you can avoid calling the API with `null`, a simple null check around the calls can be a easy work around.
4. However, some APIs require passing in null to turn off or disable some features. In these cases, you can work around the issue by bringing up the assembly browser (see [Finding the C# member for a given selector](~/mac/app-fundamentals/mac-apis.md#finding_selector)), copying the binding, and removing the null check. Please make sure to file a bug (step 2) if you do this, as your copied binding won't receive updates and fixes that we make in Xamarin.Mac, and this should be considered a short term work around.

<a name="reporting-bugs"></a>

## Reporting bugs

Your feedback is important to us. If you find any problems with Xamarin.Mac:

- Check the [Xamarin.Mac Forums](https://forums.xamarin.com/categories/xamarin-mac)
- Search the [issue repository](https://github.com/xamarin/xamarin-macios/issues)
- Before switching to GitHub issues, Xamarin issues were tracked on [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi). Please search there for matching issues.
- If you cannot find a matching issue, please file a new issue in the [GitHub issue repository](https://github.com/xamarin/xamarin-macios/issues/new).

GitHub issues are all public. It’s not possible to hide comments or attachments.

Please include as much of the following as possible:

- A simple example reproducing the issue. This is **invaluable** where possible.
- The full stack trace of the crash.
- The C# code surrounding the crash.
