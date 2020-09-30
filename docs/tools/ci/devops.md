---
title: "DevOps with Xamarin"
ms.assetid: ff978cc2-5a25-46d6-921b-e51adaa65992
author: davidortinau
ms.author: daortin
manager: crdun
ms.workload:
  - "xamarin"
ms.date: 10/23/2018
---
# DevOps with Xamarin

Xamarin enables you to build cross-platform mobile apps targeting Android, iOS, and Windows using C#, .NET, and Visual Studio. Xamarin allows a large portion of code to be shared between platforms, with only a small percentage needing to be platform-specific.

Developing apps for modern platforms involves many more activities than just writing code. These activities, referred to as DevOps (development + operations), span the app's complete life cycle and include planning and tracking work, designing and implementing code, managing a source code repository, running builds, managing continuous integrations and deployments, testing (including unit tests and UI tests), running various forms of diagnostics in both development and production environments, and monitoring app performance and user behaviors in real time through telemetry and analytics.

Visual Studio, together with Azure DevOps Services and Team Foundation Server, provides a variety of DevOps capabilities. Many of these are wholly applicable to cross-platform projects. This is especially true with Xamarin apps, because they're built with C# and .NET, around which some DevOps tools are built. Other tools require tight integration with build and runtime environments. Because Xamarin apps run on non-Windows platforms and use the Mono implementation of .NET, Xamarin provides specialized tools for certain needs.

The following tables identify which DevOps features in Visual Studio you can expect to work well with a Xamarin project, and which ones have limitations. Refer to the linked documentation for details on the features themselves.

## Agile tools

Reference link: **[About Agile tools and Agile project management](/azure/devops/boards/backlogs/backlogs-overview?view=azure-devops)**

General Comment: all planning and tracking features are independent of project type and coding languages.

|Feature|Supported with Xamarin|Additional Comments|
|-------------|----------------------------|-------------------------|
|Manage backlogs and sprints|Yes||
|Work tracking|Yes||
|Team room collaboration|Yes||
|Kanban boards|Yes||
|Report and visualize progress|Yes||

## Modeling

Reference link: **[Analyze and model architecture](/visualstudio/modeling/analyze-and-model-your-architecture)**

Design features are independent of coding language, or work with .NET languages like C#. See [Roles of architecture and modeling diagrams in software development](/visualstudio/modeling/scenario-change-your-design-using-visualization-and-modeling#ModelingDiagramsTools) for what aspects are related to code.

|Feature|Supported with Xamarin|Additional Comments|
|-------------|----------------------------|-------------------------|
|Sequence diagrams|Yes||
|Dependency graphs|Yes||
|Call hierarchy|Yes||
|Class designer|Yes||
|Architecture explorer|Yes||
|UML diagrams (use case, activity, class, component, sequence, and DSL)|Yes||
|Layer diagrams|Yes||
|Layer validation|Yes||

## Code

|Feature|Supported with Xamarin|Additional Comments|
|-------------|----------------------------|-------------------------|
|[Use Team Foundation Version Control (TFVC)](/azure/devops/repos/tfvc/overview?view=vsts) or Azure Repos|Yes||
|[Getting started with Git in Azure Repos](/azure/devops/repos/git/gitquickstart?view=vsts&tabs=visual-studio)|Yes||
|[Improve Code Quality](/visualstudio/test/improve-code-quality)|Yes||
|[Find code changes and other history](/visualstudio/ide/find-code-changes-and-other-history-with-codelens)|Yes|Except across platform-specific boundaries where the implementation isn't resolved until run time.|
|[Use code maps to debug your applications](/visualstudio/modeling/use-code-maps-to-debug-your-applications)|Yes||

## Build

Reference link: **[Azure Pipelines](/azure/devops/pipelines/index?view=vsts)**

|Feature|Supported with Xamarin|Additional Comments|
|-------------|----------------------------|-------------------------|
|On-premises TFS server|Yes|Build machines must have Xamarin installed and can be linked to an OSX computer to build for iOS. See [Use TFVC](/azure/devops/repos/tfvc/overview?view=vsts)|
|On-premises build server linked to Azure Pipelines|Yes|See [Build and release agents](/azure/devops/pipelines/agents/agents?view=vsts) for instructions.|
|Hosted controller service of Azure Pipelines|Yes|See [Build your Xamarin app](/azure/devops/pipelines/languages/xamarin?view=vsts&tabs=vsts).|
|Build definitions with pre- and post-scripts|Yes||
|Continuous integration including gated check-ins|Yes|Gated check-ins for TFVC only as Git works on a pull-request model rather than check-ins.|

## Test

|Feature|Supported with Xamarin|Additional Comments|
|-------------|----------------------------|-------------------------|
|Planning tests, creating test cases and organizing test suites|Yes||
|Manual testing|Yes||
|Test Manager (record and playback tests)|Yes|Windows devices and Android emulators only from Visual Studio.|
|Code coverage|n/a||
|[Unit test your code](/visualstudio/test/unit-test-your-code/)|Yes|For Windows and Android targets, the built-in MSTest tools can be used. To run unit tests on Windows, Android, and iOS, Xamarin recommends NUnit. See [Use TFVC](/azure/devops/repos/tfvc/overview?view=vsts).|
|[Use UI automation to test your code](/visualstudio/test/use-ui-automation-to-test-your-code/)|Windows only|Visual Studio's UI test recorder is Windows only. For all platforms, see [Xamarin.UITest](/appcenter/test-cloud/uitest/).|

## Improve code quality

Reference link: **[Improve Code Quality](/visualstudio/test/improve-code-quality)**

|Feature|Supported with Xamarin|Additional Comments|
|-------------|----------------------------|-------------------------|
|[Analyze managed code quality](/visualstudio/code-quality/analyzing-managed-code-quality-by-using-code-analysis)|Yes||
|[Find duplicate code by using code clone detection](/previous-versions/hh205279(v=vs.140))|Yes||
|[Measure complexity and maintainability of managed code](/visualstudio/code-quality/measuring-complexity-and-maintainability-of-managed-code)|Yes||
|[Performance Explorer](/visualstudio/profiling/performance-explorer)|No|Use the [Xamarin Profiler](../profiler/index.md) through Visual Studio for Mac instead. Note that the Xamarin Profiler is currently in preview and does not yet work for Windows targets.|
|[Analyze .NET Framework memory issues](/visualstudio/misc/analyze-dotnet-framework-memory-issues)|No|Visual Studio tools do not have hooks into the Mono framework for profiling.|

## Release management

Reference link: **[Build and release in Azure Pipelines and TFS](/azure/devops/pipelines/overview?view=vsts)**

|Feature|Supported with Xamarin|Additional Comments|
|-------------|----------------------------|-------------------------|
|Manage release processes|Yes||
|Deployment to servers for side-loading via scripts|Yes||
|Upload to app store|Partial|Extensions are available that can automate this process for some app stores.  See [Extensions for Azure DevOps Services](https://marketplace.visualstudio.com/VSTS); for example, the [extension for Google Play](https://marketplace.visualstudio.com/items?itemName=ms-vsclient.google-play).|

## Monitor with HockeyApp

Reference link: **[Monitor with HockeyApp](https://www.hockeyapp.net/features/)**

|Feature|Supported with Xamarin|Additional Comments|
|-------------|----------------------------|-------------------------|
|Crash analytics, telemetry, and beta distribution|Yes||