---
title: "How do I perform a thorough uninstall for Xamarin for Visual Studio?"
description: Learn how to execute a complete uninstall for Xamarin in Visual Studio.
ms.topic: troubleshooting
ms.service: xamarin
ms.assetid: c1742239-05ea-449d-9c99-611e5e5a90e4
author: davidortinau
ms.author: daortin
ms.date: 12/02/2016
no-loc: [Objective-C]
---

# How do I perform a thorough uninstall for Xamarin for Visual Studio?

1. From the Windows Control Panel, uninstall any of the following that are present:

    - Xamarin
    - Xamarin for Windows
    - Xamarin.Android
    - Xamarin.iOS
    - Xamarin for Visual Studio

2. In Explorer, delete any remaining files from the Xamarin Visual Studio extension folders (all versions, including both _Program Files_ and _Program Files (x86)_):

    _C:\\Program Files\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\Extensions\\Xamarin_

3. Delete Visual Studio's MEF component cache directory as well:

    _%LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*.0\\ComponentModelCache_

    In fact, this step by itself is often sufficient to resolve errors such as:

    - "The 'XamarinShellPackage' package did not load correctly"

    - "The project file ... cannot be opened. There is a missing project subtype"

    - "Object reference not set to an instance of an object.  at Xamarin.VisualStudio.IOS.XamarinIOSPackage.Initialize()"

    - "SetSite failed for package" (in Visual Studio's _ActivityLog.xml_)

    - "LegacySitePackage failed for package" (in Visual Studio's _ActivityLog.xml_)

    (See also the [Clear MEF Component Cache](https://visualstudiogallery.msdn.microsoft.com/22b94661-70c7-4a93-9ca3-8b6dd45f47cd) Visual Studio extension.  And see [Bug 40781, Comment 19](https://bugzilla.xamarin.com/show_bug.cgi?id=40781#c19) for a bit more context about the upstream issue in Visual Studio that can cause these errors.)

4. Also check in the _VirtualStore_ directory to see if Windows might have stored any overlay files for the _Extensions\\Xamarin_ or _ComponentModelCache_ directories there:

    _%LOCALAPPDATA%\\VirtualStore_

5. Open the registry editor (`regedit`).

6. Look for the following key:

    _HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\Windows\\CurrentVersion\\SharedDlls_

7. Find and delete any entries that match this pattern:

    _C:\\Program Files\*\\Microsoft Visual Studio 1\*.0\\Common7\\IDE\\Extensions\\Xamarin_

8. Look for this key:

    _HKEY\_CURRENT\_USER\\Software\\Microsoft\\VisualStudio\\1\*.0\\ExtensionManager\\PendingDeletions_

9. Delete any entries that look like they might be related to Xamarin.  For example, here's one that used to cause trouble in older versions of Xamarin:

    _Mono.VisualStudio.Shell,1.0_

10. Open an administrator `cmd.exe` command prompt, and then run the `devenv /setup` and `devenv /updateconfiguration` commands for each installed version of Visual Studio.  For example, for Visual Studio 2015:

    ```
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /setup
    "%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\devenv.exe" /updateconfiguration
    ```

11. Reboot.

12. Reinstall the current stable version of Xamarin using from [visualstudio.com](https://visualstudio.com/xamarin/).

## Additional troubleshooting steps for "package did not load correctly"

In cases where the above steps do not resolve the "package did not load correctly" error, here are a few more steps to try.

1. Create a new Windows user account.

2. Check if the Xamarin Visual Studio extensions load without error for the new user.

3. If the extensions load correctly, then the problem is most likely caused by some of the stored settings for the original user:

    - **In Explorer** –  _%LOCALAPPDATA%\\Microsoft\\VisualStudio\\1\*.0_
    - **In regedit** – _HKEY\_CURRENT\_USER\\Software\\Microsoft\\VisualStudio\\1\*.0_
    - **In regedit** – _HKEY\_CURRENT\_USER\\Software\\Microsoft\\VisualStudio\\1\*.0\_Config_

4. If those stored settings do indeed appear to be the problem, you can try backing them up and then deleting them.
