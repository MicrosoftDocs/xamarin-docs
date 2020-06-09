---
title: "Using mtouch to Bundle Xamarin.iOS Apps"
description: "This document describes mtouch, a tool that drives many of the steps required to turn a Xamarin.iOS application into a bundle, launch it in the simulator, and deploy it to a physical device."
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/05/2017
---

# Using mtouch to Bundle Xamarin.iOS Apps

iPhone applications are shipped as application bundles. These are directories
with the extension `.app` that contain your code, data, configuration
files and a manifest that the iPhone uses to learn about your application.

The process of turning a .NET executable into an application is mostly driven
by the `mtouch` command, a tool that integrates many of the steps
required to turn the application into a bundle. This tool is also used to launch
your application in the simulator and to deploy the software to an actual iPhone
or iPod Touch device.

## Detailed Instructions

Check our [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) manual page with all of the possible uses of the mtouch
tool.

## Installation

On a Mac, `mtouch` is bundled with Xamarin.iOS. It can be found in the 
following directory:

**/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**

To make `mtouch` convenient to use, add its parent directory to your 
system's `PATH` environment variable.  

For example, to do this in Bash, add the following line to the end of your 
**~/.bash_profile** file:

```bash
export PATH=$PATH:/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin
```

> [!WARNING]
> To use `mtouch`, do not rely on the existence of **/Developer/MonoTouch/usr/bin**, 
> a symbolic link that points to 
> **/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin**. This
> symbolic link exists only to maintain compatibility with older MonoTouch
> releases that were not installed in **/Library/Frameworks/...**, and it 
> may disappear in a future release.

## Building

The `mtouch` command can compile your code in three different
ways:

- Compile for simulator testing.
- Compile for device deployment.
- Deploy your executable to the device.

### Building for the Simulator

When you get started, the most common used scenario will be for you to try
out the application in the Simulator, so you will be using the `mtouch -sim` to compile the code into a simulator package. This is done like
this:

```bash
$ mtouch -sim Hello.app hello.exe
```

### Building for the Device

To build software for the device you will build your application using the `mtouch -dev` option, additionally you need to provide the name of
the certificate used to sign your application. The following shows how the
application is built for the device:

```bash
$ mtouch -dev -c "iPhone Developer: Miguel de Icaza" foo.exe
```

In this particular case, we are using the "iPhone Developer: Miguel de Icaza"
certificate to sign the application. This step is very important, or the
physical device will refuse to load the application.

 <a name="Running_your_Application"></a>

## Running your Application

### Launching on the Simulator

Launching on the simulator is very simple once you have an application
bundle:

```bash
$ mtouch --sdkroot /Applications/Xcode.app -launchsim Hello.app 
```

If the `--sdkroot` flag is not set it will defaults to xcode-select path and result in the following warning:

> eg: warning MT0061: No Xcode.app specified (using --sdkroot), using the system Xcode as reported by 'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer 

The command line above will produce some output like this:

```bash
Launching application
Application launched
PID: 98460
Press enter to terminate the application
```

It is strongly recommended that you also keep a log of the standard output
and standard error files to assist in your debugging. The output of
`Console.WriteLine` goes to `stdout`, and the output from `Console.Error.WriteLine`
and any other runtime error messages goes to `stderr`.

To do this, use the `--stdout` and `--stderr` flags:

```bash
../../tools/mtouch/mtouch --launchsim=Hello.app --stdout=output --stderr=error
```

If your application fails, you can see the output and error to diagnose
the problem.

### Deploying to a Device

To deploy to your device you need to provision your device as described in
Apple's [Managing Devices](https://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/00-About_the_iOS_Application_Development_Workflow/introduction.html) document. Once your device has been
properly provisioned, you can use the mtouch command to deploy a compiled ".app"
into your device. You do this using this command:

```bash
$ mtouch —sdkroot /Applications/Xcode.app -installdev=MyApp.app
```

If the `--sdkroot` flag is not set it will defaults to xcode-select path and result in the following warning:

> eg: warning MT0061: No Xcode.app specified (using --sdkroot), using the system Xcode as reported by 'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer 

These steps are typically performed by Visual Studio for Mac.

## Reference

See the [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)) manual page for details on the other command line
options.

