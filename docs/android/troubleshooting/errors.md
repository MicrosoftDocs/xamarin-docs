---
title: "Xamarin.Android Errors Matrix"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
---

# Xamarin.Android Errors Matrix

## Errors Reference

This document provides some information on the various error codes from Xamarin.

<table>
    <thead>
        <tr>
            <td>Category</td>
            <td>Description</td>
        </tr>
    </thead>
    <tbody>
        <tr><td>XA0xxx</td><td><code>mandroid</code> Errors</td></tr>
        <tr><td>XA1xxx</td><td>File copy / symlinks (project related) Errors</td></tr>
        <tr><td>XA2xxx</td><td>Linker Errors</td></tr>
        <tr><td>XA3xxx</td><td>AOT Errors</td></tr>
        <tr><td>XA4xxx</td><td>Code Generation Errors</td></tr>
        <tr><td>XA5xxx</td><td>GCC and Toolchain Errors</td></tr>
        <tr><td>XA6xxx</td><td><code>mandroid</code> Internal Tools errors</td></tr>
        <tr><td>XA7xxx</td><td>Reserved</td></tr>
        <tr><td>XA8xxx</td><td>Reserved</td></tr>
        <tr><td>XA9xxx</td><td>Licensing Errors</td></tr>
    </tbody>
</table>

## Error Codes

### XA0xxx Errors

<table>
    <thead>
        <tr><td>Error Code</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA0000</td><td>Unexpected error - Please fill a bug report at <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA0001</td><td>'-devname was provided without any device-specific action</td></tr>
        <tr><td>XA0002</td><td>Could not parse the environment variable '{0}'.</td></tr>
        <tr><td>XA0003</td><td>Application name '{0}.exe' conflicts with an SDK or product assembly (.dll) name.</td></tr>
        <tr><td>XA0004</td><td>New refcounting logic requires sgen to be enabled too.</td></tr>
        <tr><td>XA0005</td><td>The output directory '{0}' does not exist</td></tr>
        <tr><td>XA0006</td><td>There is no devel platform at '{0}', use --platform=PLAT to specify the SDK</td></tr>
        <tr><td>XA0007</td><td>The root assembly '{0}' does not exist</td></tr>
        <tr><td>XA0008</td><td>You should provide one root assembly only</td></tr>
        <tr><td>XA0009</td><td>Error while loading assemblies: {0}</td></tr>
        <tr><td>XA0010</td><td>Could not parse the command line arguments: {0}</td></tr>
        <tr><td>XA0011</td><td>{0} was built against a more recent runtime ({1}) than MonoTouch supports</td></tr>
        <tr><td>XA0012</td><td>Incomplete data is provided to complete `{0}`.</td></tr>
        <tr><td>XA0013</td><td>Profiling support requires sgen to be enabled too</td></tr>
        <tr><td>XA0014</td><td>iOS {0} does not support building applications targeting ARMv6</td></tr>
        <tr><td>XA0020</td><td>Could not determine mandroid path.</td></tr>
        <tr><td>XA0100</td><td>EmbeddedNativeLibrary '{0}' is invalid in Android Application project. Please use AndroidNativeLibrary instead.</td></tr>
    </tbody>
</table>

### XA1xxx Errors

<table>
    <thead>
        <tr><td>Error Code</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA1001</td><td>Could not find an application at the specified directory</td></tr>
        <tr><td>XA1002</td><td>Could not create symlinks, files were copied</td></tr>
        <tr><td>XA1003</td><td>Could not kill the application '{0}'. You may have to kill the application manually.</td></tr>
        <tr><td>XA1004</td><td>Could not get the list of installed applications.</td></tr>
        <tr><td>XA1005</td><td>Could not kill the application '{0}' on the device '{1}': {2}. You may have to kill the application manually.</td></tr>
        <tr><td>XA1006</td><td>Could not install the application '{0}' on the device '{1}': {2}.</td></tr>
        <tr><td>XA1007</td><td>Failed to launch the application '{0}' on the device '{1}': {2}. You can still launch the application manually by tapping on it.</td></tr>
        <tr><td>XA1008</td><td>Failed to launch the simulator: {0}</td></tr>
        <tr><td>XA1009</td><td>Could not copy the assembly '{0}' to '{1}': {2}</td></tr>
        <tr><td>XA1010</td><td>Could not load the assembly '{0}': {1}</td></tr>
        <tr><td>XA1011</td><td>Could not add missing resource file: '{0}'</td></tr>
        <tr><td>XA1101</td><td>Could not start app</td></tr>
        <tr><td>XA1102</td><td>Could not attach to the app (to kill it): {0}</td></tr>
        <tr><td>XA1103</td><td>Could not detach</td></tr>
        <tr><td>XA1104</td><td>Failed to send packet: {0}</td></tr>
        <tr><td>XA1105</td><td>Unexpected response type</td></tr>
        <tr><td>XA1106</td><td>Could not get list of applications on the device: Request timed out.</td></tr>
        <tr><td>XA1107</td><td>Application failed to launch</td></tr>
        <tr><td>XA1201</td><td>Could not load the simulator: {0}</td></tr>
        <tr><td>XA1301</td><td>Native library `{0}` ({1}) was ignored since it does not match the current build architecture(s) ({2})</td></tr>
    </tbody>
</table>

### XA2xxx Errors

<table>
    <thead>
        <tr><td>Error Code</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA2001</td><td>Could not link assemblies</td></tr>
        <tr><td>XA2002</td><td>Can not resolve reference: {0}</td></tr>
        <tr><td>XA2003</td><td>Option '{0}' will be ignored since linking is disabled</td></tr>
        <tr><td>XA2004</td><td>Extra linker definitions file '{0}' could not be located.</td></tr>
        <tr><td>XA2005</td><td>Definitions from '{0}' could not be parsed.</td></tr>
        <tr><td>XA2006</td><td>Reference to metadata item '{0}' (defined in '{1}') from '{2}' could not be resolved.</td></tr>
    </tbody>
</table>

### XA3xxx Errors

These are AOT errors.

<table>
    <thead>
        <tr><td>Error Code</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA3001</td><td>Could not AOT the assembly '{0}'</td></tr>
        <tr><td>XA3002</td><td>AOT restriction: Method '{0}' must be static since it is decorated with [MonoPInvokeCallback].</td></tr>
        <tr><td>XA3003</td><td>Conflicting --debug and --llvm options. Soft-debugging is disabled.</td></tr>
    </tbody>
</table>

### XA4xxx Errors

These are code generation errors.

<table>
    <thead>
        <tr><td>Error Code</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA4001</td><td>The main template could not be expansed to `{0}`.</td></tr>
        <tr><td>XA4101</td><td>The registrar cannot build a signature for type `{0}`.</td></tr>
        <tr><td>XA4102</td><td>The registrar found an invalid type `{0}` in signature for method `{2}`. Use `{1}` instead.</td></tr>
        <tr><td>XA4103</td><td>The registrar found an invalid type `{0}` in signature for method `{2}`: The type implements INativeObject, but does not have a constructor that takes two (IntPtr, bool) arguments</td></tr>
        <tr><td>XA4104</td><td>The registrar cannot marshal the return value for type `{0}` in signature for method `{1}`.</td></tr>
        <tr><td>XA4105</td><td>The registrar cannot marshal the parameter of type `{0}` in signature for method `{1}`.</td></tr>
        <tr><td>XA4106</td><td>The registrar cannot marshal the return value for structure `{0}` in signature for method `{1}`.</td></tr>
        <tr><td>XA4107</td><td>The registrar cannot marshal the parameter of type `{0}` in signature for method `{1}`.</td></tr>
        <tr><td>XA4108</td><td>The registrar cannot get the ObjectiveC type for managed type `{0}`.</td></tr>
        <tr><td>XA4109</td><td>Failed to compile the generated registrar code. Please file a bug report at <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA4110</td><td>The registrar cannot marshal the out parameter of type `{0}` in signature for method `{1}`.</td></tr>
        <tr><td>XA4111</td><td>The registrar cannot build a signature for type `{0}' in method `{1}`.</td></tr>
        <tr><td>XA4200</td><td>Can only generate ACW's for 'claas' types.</td></tr>
        <tr><td>XA4201</td><td>Unable to determine JNI name for type {0}.</td></tr>
        <tr><td>XA4203</td><td>The specified type name must be fully qualified.</td></tr>
        <tr><td>XA4204</td><td>Unable to resolve interface type '{0}'. Are you missing an assembly reference?</td></tr>
        <tr><td>XA4205</td><td>[ExportField] can only be used on methods with 0 parameters.</td></tr>
        <tr><td>XA4206</td><td>[Export] cannot be used on a generic type.</td></tr>
        <tr><td>XA4207</td><td>[ExportField] cannot be used on a generic type.</td></tr>
        <tr><td>XA4208</td><td>[Java.Interop.ExportFieldAttribute] cannot be used on a method returning void.</td></tr>
        <tr><td>XA4209</td><td>Failed to create JavaTypeInfo for class: {0} due to {1}</td></tr>
        <tr><td>XA4210</td><td>You need to add a reference to Mono.Android.Export.dll when you use ExportAttribute or ExportFieldAttribute.</td></tr>
        <tr><td>XA4211</td><td>AndroidManifest.xml //uses-sdk/@android:targetSdkVersion '{0}' is less than $(TargetFrameworkVersion) '{1}'. Using API-{1} for ACW compilation.</td></tr>
    </tbody>
</table>

### XA5xxx Errors

<table>
    <thead>
        <tr><td>Error Code</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA5101</td><td>Missing '{0}' compiler. Please install Android NDK.</td></tr>
        <tr><td>XA5102</td><td>Conversion from assembly to native code failed. Please file a bug report at <a href="http://bugzilla.xamarin.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5103</td><td>Failed to compile the file '{0}'. Please file a bug report at <a href="http://bugzilla.com">http://bugzilla.xamarin.com</a></td></tr>
        <tr><td>XA5201</td><td>Native linking failed. Please review user flags provided to gcc: {0}</td></tr>
        <tr><td>XA5202</td><td>Native linking failed. Please review the build log.</td></tr>
        <tr><td>XA5303</td><td>Native linking warning: {0}</td></tr>
        <tr><td>XA5300</td><td>Android SDK not found or not fully installed.</td></tr>
        <tr><td>XA5301</td><td>Missing 'strip' tool. Please install Xcode 'Command-Line Tools' component</td></tr>
        <tr><td>XA5302</td><td>Missing 'dsymutil' tool. Please install Xcode 'Command-Line Tools' component</td></tr>
        <tr><td>XA5203</td><td>Failed to generate the debug symbols (dSYM directory). Please review the build log.</td></tr>
        <tr><td>XA5204</td><td>Failed to strip the final binary. Please review the build log.</td></tr>
        <tr><td>XA5205</td><td>Missing 'aapt' tool. Please install the Android SDK Build-tools package.</td></tr>
        <tr><td>XA5206</td><td>{0}. Android resource directory {1} doesn't exist.</td></tr>
        <tr><td>XA5207</td><td>{0}. Java library file {1} doesn't exist.</td></tr>
        <tr><td>XA5208</td><td>Download failed. Please download {0} and put it to the {1} directory.</td></tr>
        <tr><td>XA5209</td><td>Unzipping failed. Please download {0} and extract it to the {1} directory.</td></tr>
        <tr><td>XA5210</td><td>{0}. Native library file {1} doesn't exist.</td></tr>
    </tbody>
</table>

### XA6xxx Errors

<table>
    <thead>
        <tr><td>Error Code</td><td>Description</td></tr>
    </thead>
    <tbody>
        <tr><td>XA6001</td><td>Running version of Cecil doesn't support assembly stripping</td></tr>
        <tr><td>XA6002</td><td>Could not strip assembly `{0}`.</td></tr>
        <tr><td>XA6003</td><td>UnauthorizedAccessException message]</td></tr>
    </tbody>
</table>

### XA9xxx Errors

This error codes are licensing and activation errors.

 <a name="xa9000" />


#### XA9000

 **Cause:** License expired

 **Checked During:** Build

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WARNING</td>
            <td>WARNING</td>
            <td>WARNING</td>
            <td>WARNING</td>
            <td>WARNING</td>
        </tr>
    </tbody>
</table>

 <a name="XA9001" />


#### XA9001

 **Cause:** Trial Expired

 **Checked During:** Build

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WARNING</td>
            <td>WARNING</td>
            <td>WARNING</td>
            <td>WARNING</td>
            <td>WARNING</td>
        </tr>
    </tbody>
</table>

 <a name="XA9002" />


#### XA9002

 **Cause:** AndroidJavaSource

 **Checked During:** Build

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Cause:** AndroidJavaLibrary

 **Checked During:** Build

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **If a Binding assembly has the .jar embedded, then this is caught at Package time, not Build time.**

 **Cause:** AndroidNativeLibrary

 **Checked During:** Build

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9003" />


#### XA9003

 **Cause:** System.Runtime.Serialization

 **Checked During:** Package

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Cause:** System.ServiceModel.Web

 **Checked During:** Package

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **Cause:** Mono.Data.Tds

 **Checked During:** Build

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 **This is referenced by System.Data.dll, which is permitted**

 **Cause:** Mono.Android.Export

 **Checked During:** Build

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9004" />


#### XA9004

 **Cause:** --profiling

 **Checked During:** Package

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9005" />


#### XA9005

 **Cause:** Size limit (32kb)

 **Checked During:** Package

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>OK</td>
            <td>-</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>

 <a name="XA9006" />


#### XA9006

 **Cause:** System.Data.SqlClient namespace

 **Checked During:** Package

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9008" />


#### XA9008

 **Cause:** Building from command-line

 **Checked During:** Build

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>OK</td>
            <td>OK</td>
            <td>OK</td>
        </tr>
    </tbody>
</table>

 <a name="XA9009" />


#### XA9009

 **Cause:** Missing serial number

 **Checked During:** Untestable

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9010" />


#### XA9010

 **Cause:** Invalid ProductId

 **Checked During:** Build

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

****Equivalent to XA9018

 <a name="XA9011" />


#### XA9011

 **Cause:** Failed to update license file (to new file format)

 **Checked During:** Activation

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9012" />


#### XA9012

 **Cause:** No internet

 **Checked During:** Activation

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9013" />


#### XA9013

 **Cause:** Unknown Error

 **Checked During:** Activation

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9014" />


#### XA9014

 **Cause:** Invalid Activation Code

 **Checked During:** Activation

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

 <a name="XA9017" />


#### XA9017

 **Cause:** Activation server doesn't return a valid license

 **Checked During:** Activation

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
            <td>ERROR</td>
        </tr>
    </tbody>
</table>

<a name="XA9018" />

#### XA9018

**Cause:** Invalid License

**Checked During:** Build

<table>
    <thead>
        <tr>
            <th>Starter</th>
            <th>Indie</th>
            <th>Business(Trial)</th>
            <th>Business</th>
            <th>Enterprise</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-</td>
            <td>-</td>
            <td>ERROR</td>
            <td>-</td>
            <td>-</td>
        </tr>
    </tbody>
</table>
