---
title: "Java Integration Overview"
description: "The Java ecosystem includes a diverse and immense collection of components. Many of these components can be used to reduce the time it takes to develop an Android application. This document will introduce and provide a high-level overview of some of the ways that developers can use these existing Java components to improve their Xamarin.Android application development experience."
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/18/2017
---

# Java Integration Overview

_The Java ecosystem includes a diverse and immense collection of components. Many of these components can be used to reduce the time it takes to develop an Android application. This document will introduce and provide a high-level overview of some of the ways that developers can use these existing Java components to improve their Xamarin.Android application development experience._


## Overview

Given the extent of the Java ecosystem, it is very likely that any 
given functionality required for a Xamarin.Android application has 
already been coded in Java. Because of this, it is appealing to try and 
reuse these existing libraries when creating a Xamarin.Android 
application. 

There are three possible ways to reuse Java libraries in a 
Xamarin.Android application: 

-   **Create a Java Bindings Library** &ndash; With this technique, a 
    Xamarin.Android project is used to create C# wrappers around the 
    Java types. A Xamarin.Android application can then reference the C# 
    wrappers created by this project, and then use the `.jar` file. 

-   **Java Native Interface** &ndash; The *Java Native* *Interface* (JNI) 
    is a framework that allows non-Java code (such as C++ or C#) to 
    call or be called by Java code running inside a JVM. 

-   **Port the Code** &ndash; This method involves taking the Java source 
    code, and then converting it to C#. This can be done manually, or 
    by using an automated tool such as Sharpen. 

At the core of the first two techniques is the *Java Native Interface* 
(JNI). JNI is a framework that allows applications not written in Java 
to interact with Java code running in a Java Virtual Machine. 
Xamarin.Android uses JNI to create *bindings* for C# code. 

The first technique is a more automated, declarative approach to 
binding Java libraries. It involves using either Visual Studio for Mac or a 
Visual Studio project type that is provided by Xamarin.Android &ndash; 
the Java Bindings Library. To successfully create these bindings, a 
Java Bindings Library may still require some manual modifications, but 
not as many as would a pure JNI approach. See 
[Binding a Java Library](~/android/platform/binding-java-library/index.md) for 
more information about Java Binding libraries. 

The second technique, using JNI, works at a much lower level, but can 
provide for finer control and access to Java methods that would not 
normally be accessible through a Java Binding Library. 

The third technique is radically different from the previous two: 
porting the code from Java to C#. Porting code from one language to 
another can be a very laborious process, but it is possible to reduce 
that effort with the help of a tool called *Sharpen*. Sharpen is an 
open source tool that is a Java-to-C# converter. 



## Summary

This document provided a high-level overview of some of the different 
ways that libraries from Java can be reused in a Xamarin.Android 
application. It introduced the concepts of bindings and managed 
callable wrappers, and discussed options for porting Java code to C#. 


## Related Links

- [Architecture](~/android/internals/architecture.md)
- [Binding a Java Library](~/android/platform/binding-java-library/index.md)
- [Working with JNI](~/android/platform/java-integration/working-with-jni.md)
- [Sharpen](https://github.com/slluis/sharpen)
- [Java Native Interface](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
