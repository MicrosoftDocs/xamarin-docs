---
title: "Assemblies"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0B1ACF06-65FF-49E2-B6BC-7AEC55638ED8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/07/2016
---

# Assemblies

## Supported Assemblies

This is a list of the assemblies supported by Xamarin for your Xamarin.tvOS apps.The detailed list of these is listed below.  Some notable omissions include `System.EnterpriseServices`, the ASP.NET stack and Windows.Forms.

<table align="center" border="1" cellpadding="1" cellspacing="1" width="90%">
	  <tbody>
	    <tr>
	      <td>
	        <strong>Assembly</strong>
	      </td>
	      <td>
	        <strong>Added</strong>
	      </td>
	      <td>
	        <strong>API Compatibility</strong>
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        Mono.CompilerServices.SymbolWriter.dll
	      </td>
	      <td align="center" valign="top">
	        1.0
	      </td>
	      <td valign="top">
	        For compiler writers.
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        Mono.Data.Sqlite.dll
	      </td>
	      <td align="center" valign="top">
	        1.2
	      </td>
	      <td>
	        ADO.NET provider for SQLite; see&nbsp;<a href="~/ios/data-cloud/system.data.md">limitations</a>.
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        Mono.Data.Tds.dll
	      </td>
	      <td align="center" valign="top">
	        1.2
	      </td>
	      <td>
	        TDS Protocol support; used for&nbsp;<a href="https://developer.xamarin.com/api/namespace/System.Data.SqlClient/" target="_blank">System.Data.SqlClient</a>&nbsp;support
	        within&nbsp;<a href="~/ios/data-cloud/system.data.md">System.Data</a>.
	      </td>
	    </tr>
	    <tr>
	      <td>
	        Mono.Security.dll
	      </td>
	      <td align="center" valign="top">
	        1.0
	      </td>
	      <td>
	        Cryptographic APIs.
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        monotouch.dll
	      </td>
	      <td align="center" valign="top">
	        1.0
	      </td>
	      <td>
	        This assembly contains the&nbsp;<a href="https://developer.xamarin.com/api/root/ios-unified/" target="_blank">C# binding to the CocoaTouch API</a>.
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        mscorlib.dll
	      </td>
	      <td align="center" valign="top">
	        1.0
	      </td>
	      <td>
	        <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        OpenTK.dll
	      </td>
	      <td align="center" valign="top">
	        1.0
	      </td>
	      <td>
	        The OpenGL/OpenAL object oriented APIs,&nbsp;<a href="https://developer.xamarin.com/api/namespace/OpenGLES/" target="_blank">extended to provide iPhone device support</a>.
	      </td>
	    </tr>
	    <tr>
	      <td align="left" valign="top">
	        System.dll
	      </td>
	      <td align="center" valign="top">
	        1.0
	      </td>
	      <td>
	        <p><a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>, plus types from the following namespaces:</p>
	
	        <ul>
	          <li>System.Collections.Specialized</li>
	          <li>System.ComponentModel</li>
	          <li>System.ComponentModel.Design</li>
	          <li>System.Diagnostics</li>
	          <li>System.IO.Compression</li>
	          <li>System.Net</li>
	          <li>System.Net.Cache</li>
	          <li>System.Net.Mail</li>
	          <li>System.Net.Mime</li>
	          <li>System.Net.NetworkInformation</li>
	          <li>System.Net.Security</li>
	          <li>System.Net.Sockets</li>
	          <li>System.Security.Authentication</li>
	          <li>System.Security.Cryptography</li>
	          <li>System.Timers</li>
	        </ul>
	
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.Core.dll
	      </td>
	      <td align="center" valign="top">
	        1.0
	      </td>
	      <td>
	        <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.Data.dll
	      </td>
	      <td align="center" valign="top">
	        1.2
	      </td>
	      <td>
	        <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>&nbsp;,&nbsp;<a href="~/ios/data-cloud/system.data.md">with some functionality
	        removed</a>.
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.Data.Service.Client.dll
	      </td>
	      <td align="center" valign="top">
	        3.x
	      </td>
	      <td>
	        Full oData client.
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.Drawing
	      </td>
	      <td align="center" valign="top">
	        1.0
	      </td>
	      <td>
	        <p>System.Drawing API - Classic API ONLY.</p>
	        <p><i>System.Drawing is not supported in the Unified API for the Xamarin.Mac .NET 4.5 or Mobile frameworks.</i></p>
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.Json.dll
	      </td>
	      <td align="center" valign="top">
	        1.1
	      </td>
	      <td>
	        <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.Runtime.Serialization.dll
	      </td>
	      <td align="center" valign="top">
	        ?
	      </td>
	      <td>
	        <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.ServiceModel.dll
	      </td>
	      <td align="center" valign="top">
	        1.1
	      </td>
	      <td>
	        <a href="http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services" target="_blank">WCF</a>&nbsp;stack as present in&nbsp;<a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.ServiceModel.Web.dll
	      </td>
	      <td align="center" valign="top">
	        ?
	      </td>
	      <td>
	        <a href="http://msdn.microsoft.com/en-us/library/cc838194(VS.95).aspx" target="_blank">Silverlight</a>, plus types from the following
	        namespaces:
	        <p>&nbsp;</p>
	
	        <ul>
	          <li>System</li>
	          <li>System.ServiceModel.Channels</li>
	          <li>System.ServiceModel.Description</li>
	          <li>System.ServiceModel.Web</li>
	        </ul>
	
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.Transactions.dll
	      </td>
	      <td align="center" valign="top">
	        1.2
	      </td>
	      <td>
	        <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>; part of&nbsp;<a href="~/ios/data-cloud/system.data.md">System.Data</a>&nbsp;support.
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.Web.Services
	      </td>
	      <td align="center" valign="top">
	        1.1
	      </td>
	      <td>
	        <a href="http://docs.xamarin.com/guides/cross-platform/application_fundamentals/introduction_to_web_services" target="_blank">Basic Web services</a>&nbsp;from the .NET 3.5 profile,
	        with the server features removed.
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.Xml.dll
	      </td>
	      <td align="center" valign="top">
	        1.0
	      </td>
	      <td>
	        <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a>
	      </td>
	    </tr>
	    <tr>
	      <td valign="top">
	        System.Xml.Linq.dll
	      </td>
	      <td align="center" valign="top">
	        1.0
	      </td>
	      <td>
	        <a href="http://msdn.microsoft.com/en-us/library/ms229335.aspx" target="_blank">.NET 3.5</a><br>
	        <br>
	        &nbsp;
	      </td>
	    </tr>
	  </tbody>
	</table>

<a name="Summary" />

## Portable Class Libraries

In addition to the Mac bindings, Xamarin.tvOS can consume [.NET Portable Class Libraries](~/cross-platform/app-fundamentals/pcl.md).



## Related Links

- [tvOS](https://developer.apple.com/tvos/)
- [tvOS Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App Programming Guide for tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
