---
title: "Assemblies Supported by Xamarin for tvOS"
description: "In order to help clarify the features available to tvOS applications, this document provides a list of assemblies supported by Xamarin for tvOS development."
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/07/2016
---

# Assemblies Supported by Xamarin for tvOS

## Supported Assemblies

This is a list of the assemblies supported by Xamarin for your Xamarin.tvOS apps.The detailed list of these is listed below.  Some notable omissions include `System.EnterpriseServices`, the ASP.NET stack and Windows.Forms.

|Assembly|Added|API Compatibility|
|---|---|---|
|Mono.CompilerServices.SymbolWriter.dll|1.0|For compiler writers.|
|Mono.Data.Sqlite.dll|1.2|ADO.NET provider for SQLite; see [limitations](~/ios/data-cloud/system.data.md).|
|Mono.Data.Tds.dll|1.2|TDS Protocol support; used for [System.Data.SqlClient](xref:System.Data.SqlClient) support within [System.Data](~/ios/data-cloud/system.data.md).|
|Mono.Security.dll|1.0|Cryptographic APIs.|
|monotouch.dll|1.0|This assembly contains the [C# binding to the CocoaTouch API](/dotnet/api/?view=xamarinios-10.8).|
|mscorlib.dll|1.0|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|
|OpenTK.dll|1.0|The OpenGL/OpenAL object oriented APIs, [extended to provide iPhone device support](xref:OpenGLES).|
|System.dll|1.0|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95)), plus types from the following namespaces: <ul><li>System.Collections.Specialized</li> <li>System.ComponentModel</li> <li>System.ComponentModel.Design</li> <li>System.Diagnostics</li> <li>System.IO.Compression</li> <li>System.Net</li> <li>System.Net.Cache</li> <li>System.Net.Mail</li> <li>System.Net.Mime</li> <li>System.Net.NetworkInformation</li> <li>System.Net.Security</li> <li>System.Net.Sockets</li> <li>System.Security.Authentication</li> <li>System.Security.Cryptography</li> <li>System.Timers</li></ul>|
|System.Core.dll|1.0|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|
|System.Data.dll|1.2|[.NET 3.5](/previous-versions/ms229335(v=vs.100)), [with some functionality removed](~/ios/data-cloud/system.data.md).|
|System.Data.Service.Client.dll|3.x|Full oData client.|
|System.Drawing|1.0|System.Drawing API - Classic API ONLY.<br />_System.Drawing is not supported in the Unified API for the Xamarin.Mac .NET 4.5 or Mobile frameworks._|
|System.Json.dll|1.1|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|
|System.Runtime.Serialization.dll|?|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|
|System.ServiceModel.dll|1.1|[WCF](../../../cross-platform/data-cloud/web-services/index.md) stack as present in [Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|
|System.ServiceModel.Web.dll|?|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95)), plus types from the following namespaces: <ul><li>System</li><li>System.ServiceModel.Channels</li><li>System.ServiceModel.Description</li><li>System.ServiceModel.Web</li></ul>|
|System.Transactions.dll|1.2|[.NET 3.5](/previous-versions/ms229335(v=vs.100)); part of [System.Data](../../data-cloud/system.data.md) support.|
|System.Web.Services|1.1|[Basic Web services](../../../cross-platform/data-cloud/web-services/index.md) from the .NET 3.5 profile, with the server features removed.|
|System.Xml.dll|1.0|[.NET 3.5](/previous-versions/ms229335(v=vs.100))|
|System.Xml.Linq.dll|1.0|[.NET 3.5](/previous-versions/ms229335(v=vs.100))|

<a name="Summary"></a>

## Portable Class Libraries

In addition to the Mac bindings, Xamarin.tvOS can consume [.NET Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md).

## Related Links

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App Programming Guide for tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)