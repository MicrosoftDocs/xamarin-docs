---
title: "Naming Parameters With Javadoc"
description: "This article explains how to recover parameter names in an Java Binding Project by using Javadoc generated from the Java project."
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/20/2017
---

# Naming Parameters With Javadoc

> [!IMPORTANT]
> We're currently investigating custom binding usage on the Xamarin platform. Please take [**this survey**](https://www.surveymonkey.com/r/KKBHNLT) to inform future development efforts.

_This article explains how to recover parameter names in an Java Binding Project by using Javadoc generated from the Java project._

## Overview

When binding an existing Java library, some metadata about the bound 
API is lost. In particular the names of parameters to methods. 
Parameter names will appear as `p0`, `p1`, etc. This is because the 
Java `.class` files do not preserve the parameter names that were used 
in the Java source code. 

A Xamarin.Android Java binding project can provide the parameter names 
if it has access to the Javadoc HTML from the original library. 

## Integrating Javadoc HTML into a Java Binding Project

Integrating the Javadoc HTML into a Java Binding project is a manual 
process consisting of the following steps: 

1. Download the Javadoc for the library
2. Edit the `.csproj` file and add a `<JavaDocPaths>` property:
3. Clean and rebuild the project

Once this is done, the original Java parameter names should be present 
in the APIs bound by a Java Binding Project. 

> [!NOTE]
> There is a great deal of variance in the JavaDoc
output. The .JAR binding toolchain does not support every single
possible permutation and consequently some parameter may not be
properly named.

## Summary

This article covered how use Javadoc in a Java Binding Project to 
provide meaning parameter names for bound APIs. 
