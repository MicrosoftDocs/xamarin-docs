---
title: "Android SDK Emulator Troubleshooting"
description: "How to identify and resolve Android SDK Emulator problems"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B05C3C5-E1F6-47A9-B098-C31E630194F6
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
---

# Android SDK Emulator Troubleshooting

In this article, the most common warning messages and issues with the
Android SDK Emulator (and their solutions) are explained.
 
<a name="perfwarn" />

## Performance Warnings

# [Visual Studio](#tab/vswin)

Beginning with Visual Studio 2017 version 15.4, a performance warning
dialog may be displayed when you first deploy your app to the Android
SDK Emulator. These warning dialogs are explained below.

### Computer Does Not Contain an Intel Procesor

![Computer does not contain an Intel processor](troubleshooting-images/01-no-intel-processor.png)

When this dialog is displayed, your computer does not have an Intel
processor, which is required for acceleration of the Android SDK
Emulator. If your computer does not have an Intel processor, we
recommend using a physical Android device for development.

### Hyper-V Is Installed or Active

![Hyper-V is installed or active](troubleshooting-images/02-hyper-v-active.png)

When this dialog is displayed, Hyper-V is installed or active and
must be disabled. [Disabling Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv) explains how to resolve this issue. 

### HAXM is Not Installed

![HAXM is not installed](troubleshooting-images/03-haxm-not-installed.png)

This dialog indicates that your computer has an Intel processor, 
Hyper-V is disabled, but HAXM is not installed.
[Installing HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm)
describes the steps for installing HAXM.

### HAXM Process Not Running

![HAXM process is not running](troubleshooting-images/04-haxm-process-not-running.png)

This dialog is displayed if your computer has an Intel processor,
Hyper-V is disabled, Intel HAXM is installed, but the HAXM process is
not running. To resolve this issue, open a Command Prompt window and
enter the following command:

```cmd
sc query intelhaxm
```

If the HAXM process is running, you should see output similar
to the following:

```cmd
SERVICE_NAME: intelhaxm
    TYPE               : 1  KERNEL_DRIVER
    STATE              : 4  RUNNING
                            (STOPPABLE, NOT_PAUSABLE, IGNORES_SHUTDOWN)
    WIN32_EXIT_CODE    : 0  (0x0)
    SERVICE_EXIT_CODE  : 0  (0x0)
    CHECKPOINT         : 0x0
    WAIT_HINT          : 0x0
```


If **STATE** is not set to **RUNNING**, see
[How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) to resolve the problem.


### Other Failures

![Other failures](troubleshooting-images/05-other-failure.png)

This dialog is displayed if your computer has an Intel processor,
Hyper-V is disabled, Intel HAXM is installed, the HAXM process is
running, but the emulator fails to start for some unknown reason.
To help resolve this error, see
[How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator).

### Disabling Performance Warnings

If you would rather not see performance warnings, you can
disable them. In Visual Studio, click **Tools > Options > Xamarin >
Android Settings** and disable the **Warn if AVD acceleration is not
supported (HAXM)** option:

[![Disabling AVD acceleration warnings](troubleshooting-images/win/06-disable-perf-warnings-sml.png)](troubleshooting-images/win/06-disable-perf-warnings.png)

# [Visual Studio for Mac](#tab/vsmac)

Beginning with Visual Studio for Mac build 7.2 (build 559) , a
performance warning dialog may be displayed when you first deploy your
app to the Android SDK Emulator. These warning dialogs are explained
below.

### HAXM is Not Installed

![HAXM is not installed](troubleshooting-images/03-haxm-not-installed.png)

This dialog indicates that HAXM is not installed.
[Installing HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm)
describes the steps for installing HAXM.

### HAXM Process Not Running

![HAXM process is not running](troubleshooting-images/04-haxm-process-not-running.png)

This dialog is displayed if the HAXM process is not running. For
detailed information to help you resolve this issue, see
[How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) to resolve the problem.

### Other Failures

![Other failures](troubleshooting-images/05-other-failure.png)

This dialog is displayed if the emulator fails to start for some
unknown reason. To help resolve this error, see
[How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/en-us/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) to resolve the problem.

-----

<a name="solutions" />

## Solutions to Common Problems

Many common Android SDK Emulator problems can be resolved by making
configuring changes to your computer or by installing additional
software. The following sections describe these issues and provide
solutions.

<a name="deployment" />

### Deployment Issues

If you get an error about a failure to install the APK on the emulator or
a failure to run the Android Debug Bridge (**adb**), verify that the
Android SDK can connect to your emulator. To do this, use the following
steps:

1. Launch the emulator from the **Android Virtual Device (AVD) Manager** (select
   your virtual device and click **Start**).

2. Open a Command Prompt and go to the folder where **adb** is
   installed. For example, on Windows, this might be at: **C:\\Program
   Files (x86)\\Android\\android-sdk\\platform-tools\\adb.exe**.

3. Type the following command:

   ```shell
   adb devices
   ```

4. If the emulator is accessible from the Android SDK, the emulator
   should appear in the list of attached devices. For example:

   ```shell
   List of devices attached
   emulator-5554   device
   ```

5. If the emulator does not appear in this list, start the **Android
   SDK Manager**, apply all updates, then try launching the
   emulator again.


<a name="haxm-issues" />

### HAXM Issues

# [Visual Studio](#tab/vswin)

If the Android SDK Emulator does not start properly, this is usually
caused by problems with HAXM. HAXM issues are often the result of
conflicts with other virtualization technologies, incorrect
settings, or an out of date HAXM driver.

<a name="virt-conflicts" />

#### HAXM Virtualization Conflicts

HAXM can conflict with other technologies that use virtualization,
such as Hyper-V, Windows Device Guard, and some antivirus software:

- **Hyper-V** &ndash; If you are using Windows with Hyper-V enabled,
  follow the steps in
  [Disabling Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-hyperv).

- **Device Guard** &ndash; Device Guard and Credential Guard can
  prevent Hyper-V from being disabled on Windows machines. To disable
  Device Guard and Credential Guard, see
  [Disabling Device Guard](~/android/get-started/installation/android-emulator/hardware-acceleration.md#disable-devguard).

- **Antivirus Software** &ndash; If you are running antivirus software
  that uses hardware-assisted virtualization (such as Avast), disable
  or uninstall this software, reboot, and retry the Android SDK
  Emulator.

<a name="bios" />

#### Incorrect BIOS Settings

If you are using HAXM on a Windows PC, HAXM will not work unless
virtualization technology (Intel VT-x) is enabled in the BIOS. If VT-x
is disabled, you will get an error similiar to the following when you
attempt to start the Android SDK Emulator:

**This computer meets the requirements for HAXM, but Intel
Virtualization Technology (VT-x) is not turned on.**

To correct this error, boot the computer into the BIOS, enable both
VT-x and SLAT (Second Level Address Translation), then restart the
computer back into Windows.

# [Visual Studio for Mac](#tab/vsmac)

If the Android SDK Emulator does not start properly, this is usually
caused by problems with HAXM. HAXM issues are often the result of
conflicts with other virtualization technologies, incorrect settings,
or an out of date HAXM driver. Try reinstalling the HAXM driver, using
the steps detailed in
[Installing HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md#install-haxm).

-----
