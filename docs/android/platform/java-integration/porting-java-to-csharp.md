---
title: "Porting Java to C#"
description: "A third option for using Java in a Xamarin.Android application is to port the Java source code to C#."
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/05/2016
---

# Porting Java to C#

_A third option for using Java in a Xamarin.Android application is to port the Java source code to C#._

## Overview

This approach may be of interest to organizations
that:

-  **Are switching technology stacks from Java to C#.**
-  **Must maintain a C# and a Java version of the same product.**
-  **Wish to have a .NET version of a popular Java library.**


There are two ways to port Java code to C#. The first way is to port the code
manually. This involves skilled developers who understand both .NET and Java and
are familiar with the proper idioms for each language. This
approach makes the most sense for small amounts of code, or for organizations
that wish to completely move away from Java to C#.

The second porting methodology is to try and automate the process by using a
code converter, such as [Sharpen](https://github.com/mono/sharpen). [Sharpen](https://github.com/mono/sharpen)
is an open source converter from
Versant that was originally used to port the code for *db4o* from Java to
C#. db4o is an object-oriented database that Versant developed in Java, and then
ported to .NET. Using a code converter may make sense for projects that must
exist in both languages and that require some parity between the two.

An example of when an automated code conversion tool makes sense can be seen
in the [ngit](https://github.com/mono/ngit) project.
Ngit is a port of the Java project [jgit](http://eclipse.org/).
Jgit itself is a Java implementation of the [Git](http://git-scm.com/) source code management
system. To generate C# code from Java, the ngit programmers use a custom
automated system to extract the Java code from jgit, apply some patches to
accommodate the conversion process, and then run Sharpen, which generates the C#
code. This allows the ngit project to benefit from the continuous, ongoing work
that is done on jgit.

There is often a non-trivial amount of work involved with bootstrapping an
automated code conversion tool, and this may prove to be a barrier to use. In
many cases, it may be simpler and easier to port Java to C# by hand.



## Related Links

- [Sharpen Conversion Tool](https://github.com/mono/sharpen)
