---
title: "Android Emulator Troubleshooting"
description: "This article explains how to diagnose and work around problems that may occur when using the Android Emulator."
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 4F053CC9-9378-47CB-8002-978A6558C4D0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/27/2018
---

# Android emulator troubleshooting

_This article describes the most common warning messages and issues
that occur while configuring and running the Android Emulator. In
addition, it describes solutions for resolving these errors as well as
various troubleshooting tips to help you diagnose emulator problems._

::: zone pivot="windows"

## Deployment issues on Windows

Some error messages may be displayed by the emulator when you deploy
your app. The most common errors and solutions are explained here.

### Deployment errors

If you see an error about a failure to install the APK on the emulator
or a failure to run the Android Debug Bridge (**adb**), verify that the
Android SDK can connect to your emulator. To verify emulator
connectivity, use the following steps:

1. Launch the emulator from the **Android Device Manager** (select
   your virtual device and click **Start**).

2. Open a command prompt and go to the folder where **adb** is
   installed. If the Android SDK is installed at its default location,
   **adb** is located at 
   **C:\\Program Files (x86)\\Android\\android-sdk\\platform-tools\\adb.exe**; 
   if not, modify this path for the location of the Android SDK on your
   computer.

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

### MMIO access error

If the message **An MMIO access error has occurred** is displayed,
restart the emulator.

<a name="gps-win" />

## Missing Google Play Services

If the virtual device you are running in the emulator does not have
Google Play Services or Google Play Store installed, this condition is
often caused by creating a virtual device without including these
packages. When you create a virtual device (see
[Managing Virtual Devices with the Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)),
be sure to select one or both of the following options:

- **Google APIs** &ndash; includes Google Play Services in the virtual device.
- **Google Play Store** &ndash; includes Google Play Store in the virtual device.

For example, this virtual device will include Google Play Services and Google Play Store:

[![Example AVD with Google Play Services and Google Play Store enabled](troubleshooting-images/win/00-add-gps-w158-sml.png)](troubleshooting-images/win/00-add-gps-w158.png#lightbox)

> [!NOTE]
> Google Play Store images are available only for some base device types such 
> as Pixel, Pixel 2, Nexus 5, and Nexus 5X.

<a name="perf-win" />

## Performance issues

Performance issues are typically caused by one of the following problems:

- The emulator is running without hardware acceleration.

- The virtual device running in the emulator is not using an x86-based system image.

The following sections cover these scenarios in more detail.

### Hardware acceleration is not enabled

If hardware acceleration is not enabled, starting a virtual device from
the Device Manager will produce a dialog with an error message
indicating that the Windows Hypervisor Platform (WHPX) is not
configured properly:

![Example Device Manager warning](troubleshooting-images/win/01-dev-mgr-warning-w158.png)

If this error message is displayed, see
[Hardware acceleration issues](#accel-issues-win) below for steps you
can take to verify and enable hardware acceleration.

### Acceleration is enabled but the emulator runs too slowly 

A common cause for this problem is not using an x86-based image in your
virtual device (AVD). When you create a virtual device (see
[Managing Virtual Devices with the Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)),
be sure to select an x86-based system image:

[![Selecting an x86 system image for a virtual device](troubleshooting-images/win/02-x86-virtual-device-w158-sml.png)](troubleshooting-images/win/02-x86-virtual-device-w158.png#lightbox)

<a name="accel-issues-win" />

## Hardware acceleration issues

Whether you are using Hyper-V or HAXM for hardware acceleration, you
may run into configuration problems or conflicts with other software on
your computer. You can verify that hardware acceleration is enabled
(and which acceleration method the emulator is using) by opening a
command prompt and entering the following command:

```cmd
"C:\Program Files (x86)\Android\android-sdk\emulator\emulator-check.exe" accel
```

This command assumes that the Android SDK is installed at the default
location of **C:\\Program Files (x86)\\Android\\android-sdk**; if not,
modify the above path for the location of the Android SDK on your
computer.

### Hardware acceleration not available

If Hyper-V is available, a message like the following example will be
returned from the **emulator-check.exe accel** command:

```cmd
HAXM is not installed, but Windows Hypervisor Platform is available.
```

If HAXM is available, a message like the following example will be
returned:

```cmd
HAXM version 6.2.1 (4) is installed and usable.
```

If hardware acceleration is not available, a message like the following
example will be displayed (the emulator looks for HAXM if it is unable
to find Hyper-V):

```cmd
HAXM is not installed on this machine
```

If hardware acceleration is not available, see
[Accelerating with Hyper-V](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vswin#hyper-v-win)
to learn how to enable hardware acceleration on your computer.

### Incorrect BIOS settings

If the BIOS has not been configured properly to support hardware
acceleration, a message similar to the following example will be 
displayed when you run the **emulator-check.exe accel** command:

```cmd
VT feature disabled in BIOS/UEFI
```

To correct this problem, reboot into your computer's BIOS and enable the
following options:

- Virtualization Technology (may have a different label depending on motherboard manufacturer).
- Hardware Enforced Data Execution Prevention.

If hardware acceleration is enabled and the BIOS is configured
properly, the emulator should run successfully with hardware acceleration.
However, problems may still result due to issues that are specific to
Hyper-V and HAXM, as explained next.

### Hyper-V issues

In some cases, enabling both **Hyper-V** and **Windows Hypervisor
Platform** in the **Turn Windows features on or off** dialog may not
properly enable Hyper-V. To verify that Hyper-V is enabled, use the
following steps:

1. Enter **powershell** in the Windows search box.

2. Right-click **Windows PowerShell** in the search results and select
   **Run as administrator**.

3. In the PowerShell console, enter the following command:

    ```powershell
    Get-WindowsOptionalFeature -FeatureName Microsoft-Hyper-V-All -Online
    ```

    If Hyper-V is not enabled, a message similar to the following example will be 
    displayed to indicate that the state of Hyper-V is **Disabled**:

    ```
    FeatureName      : Microsoft-Hyper-V-All
    DisplayName      : Hyper-V
    Description      : Provides services and management tools for creating and running virtual machines and their resources.
    RestartRequired  : Possible
    State            : Disabled
    CustomProperties : 
    ```

4. In the PowerShell console, enter the following command:

    ```powershell
    Get-WindowsOptionalFeature -FeatureName HypervisorPlatform -Online
    ```

    If the Hypervisor is not enabled, a message similar to the following example will be 
    displayed to indicate that the state of HypervisorPlatform is **Disabled**:

    ```
    FeatureName      : HypervisorPlatform
    DisplayName      : Windows Hypervisor Platform
    Description      : Enables virtualization software to run on the Windows hypervisor
    RestartRequired  : Possible
    State            : Disabled
    CustomProperties : 
    ```

If Hyper-V and/or HypervisorPlatform are not enabled, use the following
PowerShell commands to enable them:

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
Enable-WindowsOptionalFeature -Online -FeatureName HypervisorPlatform -All
```

After these commands complete, reboot. 

For more information about enabling Hyper-V (including techniques for
enabling Hyper-V using the Deployment Image Servicing and Management
tool), see
[Install Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v).

### HAXM issues

HAXM issues are often the result of conflicts with other virtualization
technologies, incorrect settings, or an out-of-date HAXM driver.

### HAXM process is not running

If HAXM is installed, you can verify that the HAXM process is running
by opening a command prompt and entering the following command:

```cmd
sc query intelhaxm
```

If the HAXM process is running, you should see output similar
to the following result:

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

If `STATE` is not set to `RUNNING`, see
[How to Use the Intel Hardware Accelerated Execution Manager](https://software.intel.com/android/articles/how-to-use-the-intel-hardware-accelerated-execution-manager-intel-haxm-android-emulator) to resolve the problem.

<a name="virt-conflicts" />

#### HAXM virtualization conflicts

HAXM can conflict with other technologies that use virtualization,
such as Hyper-V, Windows Device Guard, and some antivirus software:

- **Hyper-V** &ndash; If you are using a version of Windows before the
  **Windows 10 April 2018 update (build 1803)** and Hyper-V is enabled, 
  follow the steps in [Disabling Hyper-V](#disable-hyperv) so that
  HAXM can be enabled.

- **Device Guard** &ndash; Device Guard and Credential Guard can
  prevent Hyper-V from being disabled on Windows machines. To disable
  Device Guard and Credential Guard, see
  [Disabling Device Guard](#disable-devguard).

- **Antivirus Software** &ndash; If you are running antivirus software
  that uses hardware-assisted virtualization (such as Avast), disable
  or uninstall this software, reboot, and retry the Android
  emulator.

#### Incorrect BIOS settings

If you are using HAXM on a Windows PC, HAXM will not work unless
virtualization technology (Intel VT-x) is enabled in the BIOS. If VT-x
is disabled, you will get an error similar to the following when you
attempt to start the Android Emulator:

**This computer meets the requirements for HAXM, but Intel
Virtualization Technology (VT-x) is not turned on.**

To correct this error, boot the computer into the BIOS, enable both
VT-x and SLAT (Second-Level Address Translation), then restart the
computer back into Windows.

<a name="disable-hyperv" />

#### Disabling Hyper-V

If you are using a version of Windows before the **Windows 10 
April 2018 Update (build 1803)** and Hyper-V is enabled, you must disable
Hyper-V and reboot your computer to install and use HAXM. If you 
are using **Windows 10 April 2018 Update (build 1803)** or later, Android
Emulator version 27.2.7 or later can use Hyper-V (instead of HAXM) for
hardware acceleration, so it is not necessary to disable Hyper-V.

You can disable Hyper-V from the Control Panel by following these
steps:

1. Enter **windows features** in the Windows
   search box and select **Turn Windows features on or off** in
   the search results.

2. Uncheck **Hyper-V**:

    ![Disabling Hyper-V in the Windows Features dialog](troubleshooting-images/win/03-uncheck-hyper-v.png)

3. Restart the computer.

Alternately, you can use the following PowerShell command to disable
the Hyper-V Hypervisor:

`Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor`

Intel HAXM and Microsoft Hyper-V cannot both be active at the same
time. Unfortunately, there is no way to switch between Hyper-V
and HAXM without restarting your computer. 

In some cases, using the above steps will not succeed in disabling
Hyper-V if Device Guard and Credential Guard are enabled. If you are
unable to disable Hyper-V (or it seems to be disabled but HAXM
installation still fails), use the steps in the next section to disable
Device Guard and Credential Guard.

<a name="disable-devguard" />

#### Disabling Device Guard

Device Guard and Credential Guard can prevent Hyper-V from being
disabled on Windows machines. This situation is often a problem for
domain-joined machines that are configured and controlled by an owning
organization. On Windows 10, use the following steps to see if **Device
Guard** is running:

1. Enter **System info** in the Windows search box and select
   **System Information** in the search results.

2. In the **System Summary**, look to see if **Device Guard
   Virtualization based security** is present and is in the **Running**
   state:

   [![Device Guard is present and running](troubleshooting-images/win/04-device-guard-sml.png)](troubleshooting-images/win/04-device-guard.png#lightbox)

If Device Guard is enabled, use the following steps to disable it:

1. Ensure that **Hyper-V** is disabled (under **Turn Windows Features
   on or off**) as described in the previous section.

2. In the Windows Search Box, enter **gpedit.msc** and select the **Edit
   group policy** search result. These steps launch the **Local Group
   Policy Editor**.

3. In the **Local Group Policy Editor**, navigate to **Computer
   Configuration > Administrative Templates > System > Device Guard**:

   [![Device Guard in Local Group Policy Editor](troubleshooting-images/win/05-group-policy-editor-sml.png)](troubleshooting-images/win/05-group-policy-editor.png#lightbox)

4. Change **Turn On Virtualization Based Security** to **Disabled** (as
   shown above) and exit the **Local Group Policy Editor**.

5. In the Windows Search Box, enter **cmd**. When **Command Prompt** appears
   in the search results, right-click **Command Prompt** and select
   **Run as Administrator**.

6. Copy and paste the following commands into the command prompt window
   (if drive **Z:** is in use, pick an unused drive letter to use
   instead):

    ```cmd
    mountvol Z: /s
    copy %WINDIR%\System32\SecConfig.efi Z:\EFI\Microsoft\Boot\SecConfig.efi /Y
    bcdedit /create {0cb3b571-2f2e-4343-a879-d86a476d7215} /d "DebugTool" /application osloader
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} path "\EFI\Microsoft\Boot\SecConfig.efi"
    bcdedit /set {bootmgr} bootsequence {0cb3b571-2f2e-4343-a879-d86a476d7215}
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} loadoptions DISABLE-LSA-ISO,DISABLE-VBS
    bcdedit /set {0cb3b571-2f2e-4343-a879-d86a476d7215} device partition=Z:
    mountvol Z: /d
    ```

7. Restart your computer. On the boot screen, you should see a prompt similar to 
   the following message:

   **Do you want to disable Credential Guard?**

   Press the indicated key to disable Credential Guard as prompted.

8. After the computer reboots, check again to ensure that Hyper-V is
   disabled (as described in the previous steps).

If Hyper-V is still not disabled, the policies of your domain-joined
computer may prevent you from disabling Device Guard or Credential
Guard. In this case, you can request an exemption from your domain
administrator to allow you to opt out of Credential Guard. Alternately,
you can use a computer that is not domain-joined if you must use HAXM.

## Additional troubleshooting tips

The following suggestions are often helpful in diagnosing Android
emulator issues.

### Starting the emulator from the command line

If the emulator is not already running, you can start it from the
command line (rather than from within Visual Studio) to view its
output. Typically, Android emulator AVD images are stored at the
following location (replace *username* with your Windows user name):

**C:\\Users\\*username*\\.android\\avd**

You can launch the emulator with an AVD image from this location by
passing in the folder name of the AVD. For example, this command launches
an AVD named **Pixel_API_27**:

```cmd
"C:\Program Files (x86)\Android\android-sdk\emulator\emulator.exe" -partition-size 512 -no-boot-anim -verbose -feature WindowsHypervisorPlatform -avd Pixel_API_27 -prop monodroid.avdname=Pixel_API_27
```

This example assumes that the Android SDK is installed at the default
location of **C:\\Program Files (x86)\\Android\\android-sdk**; if not,
modify the above path for the location of the Android SDK on your
computer.

When you run this command, it will produce many lines of output while
the emulator starts up. In particular, lines such as the following
example will be printed if hardware acceleration is enabled and working
properly (in this example, HAXM is used for hardware acceleration):

```cmd
emulator: CPU Acceleration: working
emulator: CPU Acceleration status: HAXM version 6.2.1 (4) is installed and usable.
```

### Viewing Device Manager logs

Often you can diagnose emulator problems by viewing the Device Manager
logs. These logs are written to the following location:

**C:\\Users\\*username*\\AppData\\Roaming\\XamarinDeviceManager**

You can view each **DeviceManager.log** file by using a text
editor such as Notepad. The following example log entry indicates that
HAXM was not found on the computer:

```cmd
Component Intel x86 Emulator Accelerator (HAXM installer) r6.2.1 [Extra: (Intel Corporation)] not present on the system
```

::: zone-end
::: zone pivot="macos"

## Deployment issues on macOS

Some error messages may be displayed by the emulator when you deploy
your app. The most common errors and solutions are explained below.

### Deployment errors

If you see an error about a failure to install the APK on the emulator
or a failure to run the Android Debug Bridge (**adb**), verify that the
Android SDK can connect to your emulator. To verify connectivity, use
the following steps:

1. Launch the emulator from the **Android Device Manager** (select
   your virtual device and click **Start**).

2. Open a command prompt and go to the folder where **adb** is
   installed. If the Android SDK is installed at its default location,
   **adb** is located at 
   **~/Library/Developer/Xamarin/android-sdk-macosx/platform-tools/adb**; 
   if not, modify this path for the location of the Android SDK on your
   computer.

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

### MMIO access error

If **An MMIO access error has occurred** is displayed,
restart the emulator.

<a name="gps-mac" />

## Missing Google Play Services

If the virtual device you are running in the emulator does not have
Google Play Services or Google Play Store installed, this condition is
usually caused by creating a virtual device without including these
packages. When you create a virtual device (see
[Managing Virtual Devices with the Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)),
be sure to select one or both of the following:

- **Google APIs** &ndash; includes Google Play Services in the virtual device.
- **Google Play Store** &ndash; includes Google Play Store in the virtual device.

For example, this virtual device will include Google Play Services and Google Play Store:

[![Example AVD with Google Play Services and Google Play Store enabled](troubleshooting-images/mac/01-google-play-services-m75-sml.png)](troubleshooting-images/mac/01-google-play-services-m75.png#lightbox)

> [!NOTE]
> Google Play Store images are available only for some base device types such 
> as Pixel, Pixel 2, Nexus 5, and Nexus 5X.

<a name="perf-mac" />

## Performance issues

Performance issues are typically caused by one of the following problems:

- The emulator is running without hardware acceleration.

- The virtual device running in the emulator is not using an x86-based system image.

The following sections cover these scenarios in more detail.

### Hardware acceleration is not enabled

If hardware acceleration is not enabled, a dialog may pop up with a
message such as **device will run unaccelerated** when you deploy your
app to the Android emulator. If you are not certain whether hardware
acceleration is enabled on your computer (or you would like to know
which technology is providing the acceleration), see
[Hardware acceleration issues](#accel-issues-mac) below for steps you can
take to verify and enable hardware acceleration.

### Acceleration is enabled but the emulator runs too slowly 

A common cause for this problem is not using an x86-based image in your
virtual device. When you create virtual device (see
[Managing Virtual Devices with the Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)),
be sure to select an x86-based system image:

[![Selecting an x86 system image for a virtual device](troubleshooting-images/mac/02-x86-virtual-device-m75-sml.png)](troubleshooting-images/mac/02-x86-virtual-device-m75.png#lightbox)

<a name="accel-issues-mac" />

## Hardware acceleration issues

Whether you are using the Hypervisor Framework or HAXM for hardware
acceleration of the emulator, you may run into problems caused by
installation issues or an out-of-date version of macOS. The following
sections can help you resolve this issue.

<a name="hypervisor-issues" />

### Hypervisor Framework issues

If you are using macOS 10.10 or later on a newer Mac, the Android
emulator will automatically use the Hypervisor Framework for hardware
acceleration. However, some older Macs or Macs running a version of
macOS earlier than 10.10 may not provide Hypervisor Framework support.

To determine whether or not your Mac supports the Hypervisor Framework,
open a Terminal and enter the following command:

```bash
sysctl kern.hv_support
```

If your Mac supports the Hypervisor Framework, the above command will
return the following result:

```bash
kern.hv_support: 1
```

If the Hypervisor Framework is not available on your Mac, you can
follow the steps in [Accelerating with HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vsmac#haxm-mac)
to use HAXM for acceleration instead.

### HAXM issues

If the Android Emulator does not start properly, this problem is often
caused by problems with HAXM. HAXM issues are often the result of
conflicts with other virtualization technologies, incorrect settings,
or an out-of-date HAXM driver. Try reinstalling the HAXM driver, using
the steps detailed in
[Installing HAXM](~/android/get-started/installation/android-emulator/hardware-acceleration.md?tabs=vsmac#install-haxm-mac).

## Additional troubleshooting tips

The following suggestions are often helpful in diagnosing Android
emulator issues.

### Starting the emulator from the command line

If the emulator is not already running, you can start it from the
command line (rather than from within Visual Studio for Mac) to view
its output. Typically, Android emulator AVD images are stored at the
following location:

**~/.android/avd**

You can launch the emulator with an AVD image from this location by
passing in the folder name of the AVD. For example, this command
launches an AVD named **Pixel_2_API_28**:

```cmd
~/Library/Developer/Xamarin/android-sdk-macosx/emulator/emulator -partition-size 512 -no-boot-anim -verbose -feature WindowsHypervisorPlatform -avd Pixel_2_API_28 -prop monodroid.avdname=Pixel_2_API_28
```

If the Android SDK is installed at its default location, the emulator
is located in the
**~/Library/Developer/Xamarin/android-sdk-macosx/emulator** directory;
if not, modify this path for the location of the Android SDK on your
Mac.

When you run this command, it will produce many lines of output while
the emulator starts up. In particular, lines such as the following
example will be printed if hardware acceleration is enabled and working
properly (in this example, Hypervisor Framework is used for hardware
acceleration):

```cmd
emulator: CPU Acceleration: working
emulator: CPU Acceleration status: Hypervisor.Framework OS X Version 10.13
```

### Viewing Device Manager logs

Often you can diagnose emulator problems by viewing the Device Manager
logs. These logs are written to the following location:

**~/Library/Logs/XamarinDeviceManager**

You can view each **Android Devices.log** file by double-clicking it to
open it in the Console app. The following example log entry indicates
that HAXM was not found:

```cmd
Component Intel x86 Emulator Accelerator (HAXM installer) r6.2.1 [Extra: (Intel Corporation)] not present on the system
```

::: zone-end
