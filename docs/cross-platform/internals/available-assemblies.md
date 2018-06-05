---
title: "Available Assemblies"
description: "This document lists the assemblies available for use in Xamarin.iOS, Xamarin.Android, and Xamarin.Mac. It also links to documentation about .NET Standard libraries and Portable Class Libraries."
ms.prod: xamarin
ms.assetid: AEF4ED0E-391F-4FA4-9F18-842BC24C272D
author: asb3993
ms.author: amburns
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

> [!div class="mx-tdCol2BreakAll"]
> |Assembly|API Compatibility|Xamarin iOS|Xamarin Android|Xamarin Mac|
> |--------|-----------------|-----------|---------------|-----------|
> |FSharp.Core.dll| |✓|✓|✓|
> |l18N.dll|Includes CJK, MidEast, Other, Rare, West|✓|✓|✓|
> |Microsoft.CSharp.dll| |✓|✓|✓|
> |Mono.CSharp.dll| |✓|✓|✓|
> |Mono.Data.Sqlite.dll|ADO.NET provider for SQLite; see limitations.|✓|✓|✓|
> |Mono.Data.Tds.dll|TDS Protocol support; used for [System.Data.SqlClient](https://developer.xamarin.com/api/namespace/System.Data.SqlClient/) support within [System.Data](https://developer.xamarin.com/api/namespace/System.Data/).|✓|✓|✓|
> |Mono.Dynamic.&#8203;Interpreter.dll| |✓| | |
> |Mono.Security.dll|Cryptographic APIs.|✓|✓|✓|
> |monotouch.dll|This assembly contains the C# binding to the CocoaTouch API. This is only available within Classic iOS Projects.|✓| | |
> |MonoTouch.&#8203;Dialog-1.dll| |✓| | |
> |MonoTouch.&#8203;NUnitLite.dll| |✓| | |
> |mscorlib.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |OpenTK-1.0.dll|The OpenGL/OpenAL object oriented APIs, extended to provide iPhone device support.|✓|✓|✓|
> |System.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus types from the following namespaces:<br />System.Collections.Specialized<br />System.&#8203;ComponentModel<br />System.ComponentModel.Design<br />System.Diagnostics<br />System.IO<br />System.IO.Compression<br />System.IO.Compression.FileSystem<br />System.Net<br />System.Net.Cache<br />System.Net.Mail<br />System.Net.Mime<br />System.Net.&#8203;NetworkInformation<br />System.Net.Security<br />System.Net.Sockets<br />System.Runtime.&#8203;InteropServices<br />System.Runtime.Versioning<br />System.Security.&#8203;AccessControl<br />System.Security.Authentication<br />System.Security.&#8203;Cryptography<br />System.Security.Permissions<br />System.Threading<br />System.Timers|✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;Composition.dll| |✓|✓|✓|
> |System.&#8203;ComponentModel.&#8203;DataAnnotations.dll| |✓|✓|✓|
> |System.Core.dll|[Silverlight](https://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Data.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx) , with [some functionality removed](~/ios/data-cloud/system.data.md).|✓|✓|✓|
> |System.Data.&#8203;Services.&#8203;Client.dll|Full oData client.|✓|✓|✓|
> |System.IO.&#8203;Compression| |✓|✓|✓|
> |System.IO.&#8203;Compression.&#8203;FileSystem| |✓|✓|✓|
> |System.Json.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.Net.&#8203;Http.dll| |✓|✓|✓|
> |System.&#8203;Numerics.dll| |✓|✓|✓|
> |System.Runtime.&#8203;Serialization.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.dll|WCF stack as present in [Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx)|✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Internals.dll| |✓|✓|✓|
> |System.&#8203;ServiceModel.&#8203;Web.dll|[Silverlight](http://msdn.microsoft.com/library/cc838194(VS.95).aspx), plus types from the following namespaces: <br />System<br />System.ServiceModel.Channels<br />System.ServiceModel.Description<br />System.ServiceModel.Web|✓|✓|✓|
> |System.&#8203;Transactions.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx); part of [System.Data](~/ios/data-cloud/system.data.md) support.|✓|✓|✓|
> |System.Web.&#8203;Services.dll|Basic Web services from the .NET 3.5 profile, with the server features removed.|✓|✓|✓|
> |System.&#8203;Windows.dll| |✓|✓|✓|
> |System.&#8203;Xml.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.&#8203;Linq.dll|[.NET 3.5](http://msdn.microsoft.com/library/ms229335.aspx)|✓|✓|✓|
> |System.Xml.Serialization.dll| |✓|✓|✓|
> |Xamarin.iOS.dll|This assembly contains the C# binding to the CocoaTouch API. This is only used in Unified iOS Projects.|✓| | |
> |Java.Interop.dll| | |✓| |
> |Mono.Android.dll| | |✓| |
> |Mono.Android.&#8203;Export.dll| | |✓| |
> |Mono.Posix.dll| | |✓| |
> |System.&#8203;EnterpriseServices.dll| | |✓| |
> |Xamarin.Android.&#8203;NUnitLite.dll| | |✓| |
> |Mono.CompilerServices.&#8203;SymbolWriter.dll|For compiler writers.| | |✓|
> |Xamarin.Mac.dll| | | |✓|
> |System.&#8203;Drawing.dll|System.Drawing API - Classic API ONLY.System.Drawing is not supported in the Unified API for the Xamarin.Mac .NET 4.5 or Mobile frameworks.System.Drawing support can be added to iOS and OS X using the [sysdrawing-coregraphics](https://github.com/mono/sysdrawing-coregraphics) library|✓| |✓|
