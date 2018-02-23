---
title: "Tools & Commands"
description: "Overview of the tools included with Objective Sharpie, and the command line arguments to use them."
ms.topic: article
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 10/05/2015
---

# Tools & Commands

_Overview of the tools included with Objective Sharpie, and the command line arguments to use them._

<style type="text/css">
.terminal-blue { color: rgb(10,96,254); }
.terminal-green { color: rgb(12,156,26); }
.terminal-magenta { color: rgb(152,12,103); }
</style>


Once Objective Sharpie is successfully [installed](~/cross-platform/macios/binding/objective-sharpie/get-started.md),
open a terminal and familiarize yourself with the <em>commands</em>
Objective Sharpie has to offer:

<pre>$ <b>sharpie -help</b>
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, -help                Show detailed help
  -v, -version             Show version information

Telemetry Options:
  -tlm-about               Show a detailed overview of what usage and binding
                             information will be submitted to Xamarin by
                             default when using Objective Sharpie.
  -tlm-do-not-submit       Do not submit any usage or binding information to
                             Xamarin. Run 'sharpie -tml-about' for more
                             information.
  -tlm-do-not-identify     Do not submit Xamarin account information when
                             submitting usage or binding information to Xamarin
                             for analysis. Binding attempts and usage data will
                             be submitted anonymously if this option is
                             specified.

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation</pre>

Objective Sharpie provides the following tools:

<table>
  <thead>
    <tr><td>Tool</td><td>Description</td>
  </thead>
  <tbody>
    <tr><td><b>xcode</b></td><td>Provides information about the current Xcode installation and the versions of iOS and Mac SDKs that are available. We will be using this information later when we generate our bindings.</td></tr>
    <tr><td><b>pod</b></td><td>Searches for, configures, installs (in a local directory), and binds Objective-C <a href="https://cocoapods.org">CocoaPod</a> libraries available from the master Spec repository. This tool evaluates the installed CocoaPod to automatically deduce the correct input to pass to the <code>bind</code> tool below. <em><strong>New in 3.0!</strong></em></td></tr>
    <tr><td><b>bind</b></td><td>Parses the header files (<code>*.h</code>) in the Objective-C library into the <a href="~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md">initial <i>ApiDefinition.cs</i> and <i>StructsAndEnums.cs</i> files</a>.</td></tr>
    <tr><td><b>update</b></td><td>Checks for newer versions of Objective Sharpie and downloads and launches the installer if one is available.</td></tr>
    <tr><td><b>verify-docs</b></td><td>Shows detailed information about <code>[Verify]</code> attributes.</td></tr>
    <tr><td><b>docs</b></td><td>Navigates to this document in your default web browser.</td></tr>
  </tbody>
</table>

To get help on a specific Objective Sharpie tool, enter the name of the tool and the `-help` option. For example, `sharpie xcode -help` returns the following output:

<pre>$ <b>sharpie xcode -help</b>
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.</pre>

Before we can start the binding process, we need to get information about our current installed SDKs by entering the following command into the Terminal `sharpie xcode -sdks`. Your output may differ depending on which version(s) of Xcode you have installed. Objective Sharpie looks for SDKs installed in any `Xcode*.app` under the `/Applications` directory:

<pre>$ <b>sharpie xcode -sdks</b>
<span class="terminal-blue">sdk:</span> appletvos9.0    <span class="terminal-green">arch:</span> arm64
<span class="terminal-blue">sdk:</span> iphoneos9.1     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos9.0     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> iphoneos8.4     <span class="terminal-green">arch:</span> arm64   armv7
<span class="terminal-blue">sdk:</span> macosx10.11     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> macosx10.10     <span class="terminal-green">arch:</span> x86_64  i386
<span class="terminal-blue">sdk:</span> watchos2.0      <span class="terminal-green">arch:</span> armv7</pre>

From the above, we can see that we have the `iphoneos9.1` SDK installed on our
machine and it has `arm64` architecture support. We will be using this value
for all the samples in this section. With this information in place, we are ready to
parse an Objective-C library header files into the initial `ApiDefinition.cs`
and `StructsAndEnums.cs` for the Binding project.

