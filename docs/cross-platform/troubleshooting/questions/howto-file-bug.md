---
title: "When and how should I file a bug report?"
description: "This document describes when, where, and how to file a bug report. It also provides bug report best practices that enable engineers to best diagnose the problem."
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: davidortinau
ms.author: daortin
ms.date: 08/01/2018
---

# When and how should I file a bug report?

> [!TIP]
> Use the **Report a problem** menu item in Visual Studio &ndash; this will send
> diagnostic information along with your bug report to help resolve the issue.
>
> There are detailed instructions for
> [Visual Studio 2019 or Visual Studio 2017](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)
> and [Visual Studio for Mac](/visualstudio/mac/report-a-problem).
>
> You can search for existing reports on the [Visual Studio Developer Community](https://developercommunity.visualstudio.com/) website.

## File a bug if...

You have a set of steps you think the engineers will be able to use to reproduce a problem.

OR

You can carefully describe the visible symptoms of the problem, especially if you can also describe some precise circumstances related to the problem.<sup>[[1]](#note-1)</sup>

## Best practices to help address bugs quickly and efficiently

1. <a name="ref-1"></a>Search [Visual Studio Developer Community](https://developercommunity.visualstudio.com/) and the web for existing bug reports or usage suggestions that might address the problem directly.<sup>[[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2"></a>Describe the problem as clearly and concisely as possible, including a description of what happened and was expected to happen.

1. <a name="ref-3"></a>Include any relevant stack traces, error message text, or crash logs (if you use the **Report a problem** feature, these can be included automatically). <sup>[[4]](#note-4)</sup>

1. <a name="ref-4"></a>Write down any important error messages that appear in screenshot attachments as plain text too.

1. <a name="ref-5"></a>Include a small, self-contained test case that reproduces the bug with as little code as possible.  If you cannot reproduce the problem with a brand new project (created using one of the built-in templates), then please zip up a project that demonstrates the problem and attach it to the bug report.  Make the example project as simple as possible before attaching it.<sup>[[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6"></a>Describe the environment where the bug was encountered, including the operating system and [versions of Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) and any dependencies.

## Additional details

1. <a name="note-1"></a>[*^*](#ref-1) Ideally the description of the "visible symptoms" should include enough details so that other customers can confirm whether they are seeing the same problem (same error messages, same performance degradation, same stack trace from a crash, _etc._). For "precise circumstances", one good example would be if you can say something like: "I normally hit the problem 75% of the time, but if I change this one thing then I can avoid the problem completely." Another similar example of a "precise circumstance" is if downgrading to a previous version of Xamarin stops the problem.

1. <a name="note-2"></a>[*^*](#ref-2) As you would expect, snippets of error text (or any other uniquely descriptive text) are usually the best search terms. If the existing bug report is incomplete, then you are welcome to add details or file a new, better bug report.

1. <a name="note-3"></a>[*^*](#ref-3) Another good question is whether the same problem has been reported for any Java, Objective-C, or Swift apps. If so, then the problem is very likely part of Android or iOS itself rather than part of Xamarin.

1. <a name="note-4"></a>[*^*](#ref-4) A few examples of information to include:

    1. For errors that occur when building a project, please include the complete [diagnostic build output](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) on the bug report.

    1. For errors that occur when building or debugging an iOS project from Visual Studio, please run **Help > Xamarin > Zip Logs** after hitting the error and include the resulting .zip file on the bug report.

    1. For exceptions or crashes in Android or iOS apps, please include the relevant [Debug logs for Xamarin.Android and Xamarin.iOS apps](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps).

1. <a name="note-5"></a>[*^*](#ref-5) If possible for your particular problem, one option is to recreate the problem by adding a small number of files from your original solution into a brand new solution. The Xamarin team will often be able to investigate problems even on larger test cases (assuming the steps to reproduce are explained clearly), but simpler test cases give the best chance that the bug will be resolved quickly.

1. <a name="note-6"></a>[*^*](#ref-6) If it is _not_ possible to reproduce the problem by adding a small number of files to a brand new solution, then you can zip up and attach the whole solution folder for your full app. Please delete the `bin`, `obj`, `Components`, and `packages` folders to make the zip file smaller. (The IDE and the build process will usually restore or recreate the contents of these folders as needed.) You can also delete as many code and resource files from the project as you like, as long as the resulting solution still demonstrates the original problem.