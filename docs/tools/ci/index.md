---
title: "Continuous Integration with Xamarin"
description: "This document links to guides that describe continuous integration with Xamarin. Linked content provides an overview of continuous integration and discusses App Center Build, TeamCity, and Jenkins."
ms.prod: xamarin
ms.assetid: 99484E96-DC69-4697-8BBB-1B44C5CBB5ED
author: davidortinau
ms.author: daortin
ms.date: 10/23/2018
---

# Continuous Integration with Xamarin

> [!Video https://youtube.com/embed/wXgnh2Q7Uv8]

## [Introduction To Continuous Integration](~/tools/ci/intro-to-ci.md)

This section covers the different components involved with continuous integration and their relationships. It outlines the continuous integration environments that are discussed in the specific sections below.

## [DevOps with Xamarin](~/tools/ci/devops.md)

This section identifies which DevOps features in Azure and Visual Studio you can expect to work well with a Xamarin project.

## Working with Continuous Integration Environments

### [Build Xamarin apps with Azure Pipelines](/azure/devops/pipelines/languages/xamarin/)

Use Azure Pipelines to automatically build Xamarin apps for Android and iOS.

### [Build Xamarin apps using App Center](/appcenter/build/xamarin/)

Build Xamarin.iOS and Xamarin.Android solutions with App Center, straight from GitHub, Azure DevOps, or Bitbucket.

### [Build Xamarin apps with TeamCity](~/tools/ci/teamcity.md)

This guide discusses the steps involved with using TeamCity to compile mobile apps and then submit them to App Center Test.

### [Build Xamarin apps with Jenkins](~/tools/ci/jenkins-walkthrough.md)

This guide illustrates how to set up Jenkins as a continuous integration server and automate compiling mobile apps created with Xamarin. It describes how to install Jenkins on OS X, configure it, and set up jobs to compile Xamarin.iOS and Xamarin.Android apps when changes are committed to the version control system.