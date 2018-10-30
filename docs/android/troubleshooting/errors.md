---
title: "Xamarin.Android Errors Matrix"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7EBE4C01-8EFC-4B7E-97BA-D879994F59AB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/13/2018
---

# Xamarin.Android Errors Matrix

## Errors Reference

This document provides some information on the various error codes from Xamarin.

|Category|Description|
|--- |--- |
|XA0xxx|mandroid Errors|
|XA1xxx|File copy / symlinks (project related) Errors|
|XA2xxx|Linker Errors|
|XA3xxx|AOT Errors|
|XA4xxx|Code Generation Errors|
|XA5xxx|GCC and Toolchain Errors|
|XA6xxx|mandroid Internal Tools errors|
|XA7xxx|Reserved|
|XA8xxx|Reserved|
|XA9xxx|Licensing Errors|


## Error Codes

### XA0xxx Errors

|Error Code|Description|
|--- |--- |
|XA0000|Unexpected error - Please fill a [bug report](https://github.com/xamarin/xamarin-android/issues/new).|
|XA0001|'-devname was provided without any device-specific action.|
|XA0002|Could not parse the environment variable '{0}'.|
|XA0003|Application name '{0}.exe' conflicts with an SDK or product assembly (.dll) name.|
|XA0004|New refcounting logic requires sgen to be enabled too.|
|XA0005|The output directory '{0}' does not exist.|
|XA0006|There is no devel platform at '{0}', use --platform=PLAT to specify the SDK|
|XA0007|The root assembly '{0}' does not exist.|
|XA0008|You should provide one root assembly only.|
|XA0009|Error while loading assemblies: {0}.|
|XA0010|Could not parse the command line arguments: {0}.|
|XA0011|{0} was built against a more recent runtime ({1}) than MonoTouch supports.|
|XA0012|Incomplete data is provided to complete '{0}'.|
|XA0013|Profiling support requires sgen to be enabled too.|
|XA0014|iOS {0} does not support building applications targeting ARMv6.|
|XA0020|Could not determine mandroid path.|
|XA0100|EmbeddedNativeLibrary '{0}' is invalid in Android Application project. Please use AndroidNativeLibrary instead.|

### XA1xxx Errors

|Error Code|Description|
|--- |--- |
|XA1001|Could not find an application at the specified directory.|
|XA1002|Could not create symlinks, files were copied.|
|XA1003|Could not kill the application '{0}'. You may have to kill the application manually.|
|XA1004|Could not get the list of installed applications.|
|XA1005|Could not kill the application '{0}' on the device '{1}': {2}. You may have to kill the application manually.|
|XA1006|Could not install the application '{0}' on the device '{1}': {2}.|
|XA1007|Failed to launch the application '{0}' on the device '{1}': {2}. You can still launch the application manually by tapping on it.|
|XA1008|Failed to launch the simulator: {0}.|
|XA1009|Could not copy the assembly '{0}' to '{1}': {2}.|
|XA1010|Could not load the assembly '{0}': {1}.|
|XA1011|Could not add missing resource file: '{0}'.|
|XA1101|Could not start app.|
|XA1102|Could not attach to the app (to kill it): {0}.|
|XA1103|Could not detach.|
|XA1104|Failed to send packet: {0}.|
|XA1105|Unexpected response type.|
|XA1106|Could not get list of applications on the device: Request timed out.|
|XA1107|Application failed to launch.|
|XA1201|Could not load the simulator: {0}.|
|XA1301|Native library '{0}' ({1}) was ignored since it does not match the current build architecture(s) ({2}).|

### XA2xxx Errors

|Error Code|Description|
|--- |--- |
|XA2001|Could not link assemblies.|
|XA2002|Can not resolve reference: {0}.|
|XA2003|Option '{0}' will be ignored since linking is disabled.|
|XA2004|Extra linker definitions file '{0}' could not be located.|
|XA2005|Definitions from '{0}' could not be parsed.|
|XA2006|Reference to metadata item '{0}' (defined in '{1}') from '{2}' could not be resolved.|

### XA3xxx Errors

These are AOT errors.

|Error Code|Description|
|--- |--- |
|XA3001|Could not AOT the assembly '{0}'.|
|XA3002|AOT restriction: Method '{0}' must be static since it is decorated with [MonoPInvokeCallback].|
|XA3003|Conflicting --debug and --llvm options. Soft-debugging is disabled.|


### XA4xxx Errors

These are code generation errors.

|Error Code|Description|
|--- |--- |
|XA4001|The main template could not be expansed to '{0}'.|
|XA4101|The registrar cannot build a signature for type '{0}'.|
|XA4102|The registrar found an invalid type '{0}' in signature for method '{2}'. Use '{1}' instead.|
|XA4103|The registrar found an invalid type '{0}' in signature for method '{2}': The type implements INativeObject, but does not have a constructor that takes two (IntPtr, bool) arguments.|
|XA4104|The registrar cannot marshal the return value for type '{0}' in signature for method '{1}'.|
|XA4105|The registrar cannot marshal the parameter of type '{0}' in signature for method '{1}'.|
|XA4106|The registrar cannot marshal the return value for structure '{0}' in signature for method '{1}'.|
|XA4107|The registrar cannot marshal the parameter of type '{0}' in signature for method '{1}'.|
|XA4108|The registrar cannot get the ObjectiveC type for managed type '{0}'.|
|XA4109|Failed to compile the generated registrar code. Please file a [bug report](http://bugzilla.xamarin.com).|
|XA4110|The registrar cannot marshal the out parameter of type '{0}' in signature for method '{1}'.|
|XA4111|The registrar cannot build a signature for type '{0}' in method '{1}'.|
|XA4200|Can only generate ACW's for 'claas' types.|
|XA4201|Unable to determine JNI name for type {0}.|
|XA4203|The specified type name must be fully qualified.|
|XA4204|Unable to resolve interface type '{0}'. Are you missing an assembly reference?|
|XA4205|[ExportField] can only be used on methods with 0 parameters.|
|XA4206|[Export] cannot be used on a generic type.|
|XA4207|[ExportField] cannot be used on a generic type.|
|XA4208|[Java.Interop.ExportFieldAttribute] cannot be used on a method returning void.|
|XA4209|Failed to create JavaTypeInfo for class: {0} due to {1}.|
|XA4210|You need to add a reference to Mono.Android.Export.dll when you use ExportAttribute or ExportFieldAttribute.|
|XA4211|AndroidManifest.xml //uses-sdk/@android:targetSdkVersion '{0}' is less than $(TargetFrameworkVersion) '{1}'. Using API-{1} for ACW compilation.|


### XA5xxx Errors

|Error Code|Description|
|--- |--- |
|XA5101|Missing '{0}' compiler. Please install Android NDK.|
|XA5102|Conversion from assembly to native code failed. Please file a [bug report](http://bugzilla.xamarin.com).|
|XA5103|Failed to compile the file '{0}'. Please file a [bug report](http://bugzilla.xamarin.com).|
|XA5201|Native linking failed. Please review user flags provided to gcc: {0}|
|XA5202|Native linking failed. Please review the build log.|
|XA5303|Native linking warning: {0}.|
|XA5300|Android SDK not found or not fully installed.|
|XA5301|Missing 'strip' tool. Please install Xcode 'Command-Line Tools' component.|
|XA5302|Missing 'dsymutil' tool. Please install Xcode 'Command-Line Tools' component.|
|XA5203|Failed to generate the debug symbols (dSYM directory). Please review the build log.|
|XA5204|Failed to strip the final binary. Please review the build log.|
|XA5205|Missing 'aapt' tool. Please install the Android SDK Build-tools package.|
|XA5206|{0}. Android resource directory {1} doesn't exist.|
|XA5207|{0}. Java library file {1} doesn't exist.|
|XA5208|Download failed. Please download {0} and put it to the {1} directory.|
|XA5209|Unzipping failed. Please download {0} and extract it to the {1} directory.|
|XA5210|{0}. Native library file {1} doesn't exist.|

### XA6xxx Errors

|Error Code|Description|
|--- |--- |
|XA6001|Running version of Cecil doesn't support assembly stripping.|
|XA6002|Could not strip assembly '{0}'.|
|XA6003|UnauthorizedAccessException message.|

### XA9xxx Errors

These error codes are licensing and activation errors.


#### XA9000

 **Cause:** License expired

 **Checked During:** Build

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|WARNING|WARNING|WARNING|WARNING|WARNING|


#### XA9001

 **Cause:** Trial Expired

 **Checked During:** Build

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|WARNING|WARNING|WARNING|WARNING|WARNING|



#### XA9002

 **Cause:** AndroidJavaSource

 **Checked During:** Build

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|OK|OK|OK|OK|

 **Cause:** AndroidJavaLibrary

 **Checked During:** Build

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|OK|OK|OK|OK|

 **If a Binding assembly has the .jar embedded, then this is caught at Package time, not Build time.**

 **Cause:** AndroidNativeLibrary

 **Checked During:** Build

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|OK|OK|OK|OK|


#### XA9003

 **Cause:** System.Runtime.Serialization

 **Checked During:** Package

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|

 **Cause:** System.ServiceModel.Web

 **Checked During:** Package

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|

 **Cause:** Mono.Data.Tds

 **Checked During:** Build

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|

 **This is referenced by System.Data.dll, which is permitted**

 **Cause:** Mono.Android.Export

 **Checked During:** Build

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|OK|OK|OK|OK|



#### XA9004

 **Cause:** --profiling

 **Checked During:** Package

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|



#### XA9005

 **Cause:** Size limit (32kb).

 **Checked During:** Package

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|OK|-|-|-|



#### XA9006

 **Cause:** System.Data.SqlClient namespace.

 **Checked During:** Package

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|


#### XA9008

 **Cause:** Building from command-line.

 **Checked During:** Build

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|OK|OK|OK|


#### XA9009

 **Cause:** Missing serial number.

 **Checked During:** Untestable

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### XA9010

 **Cause:** Invalid ProductId.

 **Checked During:** Build

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|

Equivalent to XA9018.



#### XA9011

 **Cause:** Failed to update license file (to new file format).

 **Checked During:** Activation

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|

#### XA9012

 **Cause:** No internet

 **Checked During:** Activation

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### XA9013

 **Cause:** Unknown Error

 **Checked During:** Activation

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### XA9014

 **Cause:** Invalid Activation Code

 **Checked During:** Activation

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### XA9017

 **Cause:** Activation server doesn't return a valid license.

 **Checked During:** Activation

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|ERROR|ERROR|ERROR|ERROR|ERROR|


#### XA9018

**Cause:** Invalid License

**Checked During:** Build

|Starter|Indie|Business(Trial)|Business|Enterprise|
|--- |--- |--- |--- |--- |
|-|-|ERROR|-|-|

