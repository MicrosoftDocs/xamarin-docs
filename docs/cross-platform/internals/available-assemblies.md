---
title: "Available Assemblies"
description: "This document lists the assemblies available for use in Xamarin.iOS, Xamarin.Android, and Xamarin.Mac. It also links to documentation about .NET Standard libraries and Portable Class Libraries."
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
---

# Available Assemblies

Xamarin.iOS, Xamarin.Android, and Xamarin.Mac all ship with over a dozen assemblies. Just as Silverlight is an extended subset of the desktop .NET assemblies, Xamarin platforms is also an extended subset of several Silverlight and desktop .NET assemblies.

Xamarin platforms are not ABI compatible with existing assemblies compiled for a different profile. You must recompile your source code to generate assemblies targeting the correct profile (just as you need to recompile source code to target Silverlight and .NET 3.5 separately).

Xamarin.Mac applications can be compiled in three modes: one that uses Xamarin's curated Mobile Profile, the Xamarin.Mac .NET 4.5 Framework which allows you target existing full desktop assemblies, and an unsupported one that uses the .NET API found in a system Mono installation. For more information, please see our [Target Frameworks](~/mac/platform/target-framework.md) documentation.

## .NET Standard Libraries

In addition to the iOS, Android, and Mac bindings, Xamarin projects can consume [.NET Standard libraries](~/cross-platform/app-fundamentals/net-standard.md).

## Portable Class Libraries

Xamarin projects can also consume [.NET Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md), although this technology is being deprecated in favor of .NET Standard.

## Supported Assemblies

These are the assemblies available in the **Reference Manager > Assemblies > Framework** (Visual Studio 2017) and **Edit References > Packages** (Visual Studio for Mac), and their compatibility with Xamarin platforms.

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|API Compatibility|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |l18N.dll|Includes CJK, MidEast, Other, Rare, West|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |Microsoft.CSharp.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |Mono.CSharp.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |Mono.Data.Sqlite.dll|ADO.NET provider for SQLite; see limitations.|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |Mono.Data.Tds.dll|TDS Protocol support; used for [System.Data.SqlClient](xref:System.Data.SqlClient) support within [System.Data](xref:System.Data).|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |Mono.Dynamic.&#8203;Interpreter.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")| | |
> |Mono.Security.dll|Cryptographic APIs.|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |monotouch.dll|This assembly contains the C# binding to the CocoaTouch API. This is only available within Classic iOS Projects.|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")| | |
> |MonoTouch.&#8203;Dialog-1.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")| | |
> |MonoTouch.&#8203;NUnitLite.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")| | |
> |mscorlib.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |OpenTK-1.0.dll|The OpenGL/OpenAL object oriented APIs, extended to provide iPhone device support.|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95)), plus types from the following namespaces:<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.Core.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.Data.dll|[.NET 3.5](/previous-versions/ms229335(v=vs.100)) , with [some functionality removed](~/ios/data-cloud/system.data.md).|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.Data.&#8203;Services.&#8203;Client.dll|Full oData client.|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.IO.&#8203;Compression| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.Json.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.Net.&#8203;Http.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.&#8203;Numerics.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.&#8203;ServiceModel.dll|WCF stack as present in [Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95))|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838194(v=vs.95)), plus types from the following namespaces: <br />System<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.&#8203;Transactions.dll|[.NET 3.5](/previous-versions/ms229335(v=vs.100)); part of [System.Data](~/ios/data-cloud/system.data.md) support.|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.Web.&#8203;Services.dll|Basic Web services from the .NET 3.5 profile, with the server features removed.|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.&#8203;Windows.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.&#8203;Xml.dll|[.NET 3.5](/previous-versions/ms229335(v=vs.100))|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](/previous-versions/ms229335(v=vs.100))|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.Xml.Serialization.dll| |![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")|![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")|![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |Xamarin.iOS.dll|This assembly contains the C# binding to the CocoaTouch API. This is only used in Unified iOS Projects.|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")| | |
> |Java.Interop.dll| | |![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")| |
> |Mono.Android.dll| | |![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")| |
> |Mono.Android.&#8203;Export.dll| | |![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")| |
> |Mono.Posix.dll| | |![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")| |
> |System.&#8203;EnterpriseServices.dll| | |![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |![Xamarin.Android Supported](~/media/shared/yes.png "Xamarin.Android Supported")| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|For compiler writers.| | |![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |Xamarin.Mac.dll| | | |![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|
> |System.&#8203;Drawing.dll|System.Drawing is not supported in the Unified API for the Xamarin.Mac, .NET 4.5, or Mobile frameworks. System.Drawing support can be added to iOS and macOS using the [sysdrawing-coregraphics](https://github.com/mono/sysdrawing-coregraphics) library|![Xamarin.iOS Supported](~/media/shared/yes.png "Xamarin.iOS Supported")| |![Xamarin.Mac Supported](~/media/shared/yes.png "Xamarin.Mac Supported")|