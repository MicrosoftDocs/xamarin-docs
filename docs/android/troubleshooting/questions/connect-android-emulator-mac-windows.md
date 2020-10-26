---
title: "Is it possible to connect to Android emulators running on a Mac from a Windows VM?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7B6752BB-8E4C-4690-B275-7E425A051F45
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
---

# Is it possible to connect to Android emulators running on a Mac from a Windows VM?

To connect to the Android Emulator running on a Mac from a Windows
virtual machine, use the following steps:

> [!NOTE]
> We recommend using an Android Emulator that does not include the Google Play Store.

1. Start the emulator on the Mac.

2. Kill the `adb` server on the Mac:

    ```bash
    adb kill-server
    ```

3. Note that the emulator is listening on 2 TCP ports on the loopback
    network interface:

    ```bash
    lsof -iTCP -sTCP:LISTEN -P | grep 'emulator\|qemu'

    emulator6 94105 macuser   20u  IPv4 0xa8dacfb1d4a1b51f      0t0  TCP localhost:5555 (LISTEN)
    emulator6 94105 macuser   21u  IPv4 0xa8dacfb1d845a51f      0t0  TCP localhost:5554 (LISTEN)
    ```

    The odd-numbered port is the one used to connect to `adb`. See also
    [https://developer.android.com/tools/devices/emulator.html#emulatornetworking](https://developer.android.com/tools/devices/emulator.html#emulatornetworking).

4. _Option 1_: Use `nc`
    to forward inbound TCP packets received externally on port 5555 (or
    any other port you like) to the odd-numbered port on the loopback
    interface (**127.0.0.1 5555** in this example), and to forward the
    outbound packets back the other way:

    ```bash
    cd /tmp
    mkfifo backpipe
    nc -kl 5555 0<backpipe | nc 127.0.0.1 5555 > backpipe
    ```

    As long as the `nc` commands stay running in a Terminal window, the
    packets will be forwarded as expected. You can type Control-C in
    the Terminal window to quit the `nc` commands once you're done
    using the emulator.

    (Option 1 is usually easier than Option 2, especially if **System Preferences > Security & Privacy > Firewall** is switched on.)

    _Option 2_: Use `pfctl`
    to redirect TCP packets from port `5555` (or any other port you
    like) on the
    [Shared Networking](https://kb.parallels.com/en/4948) interface to
    the odd-numbered port on the loopback interface (`127.0.0.1:5555`
    in this example):

    ```bash
    sed '/rdr-anchor/a rdr pass on vmnet8 inet proto tcp from any to any port 5555 -> 127.0.0.1 port 5555' /etc/pf.conf | sudo pfctl -ef -
    ```

    This command sets up port forwarding using the `pf packet filter`
    system service. The line breaks are important. Be sure to keep them
    intact when copy-pasting. You will also need to adjust the
    interface name from *vmnet8* if you're using Parallels. `vmnet8` is
    the name of the special *NAT device* for the *Shared Networking*
    mode in VMWare Fusion. The appropriate network interface in
    Parallels is likely
    [vnic0](https://download.parallels.com/doc/psbm/en/Parallels_Server_Bare_Metal_Users_Guide/29258.htm).

5. Connect to the emulator from the Windows machine:

    ```cmd
    C:\> adb connect ip-address-of-the-mac:5555
    ```

    Replace "ip-address-of-the-mac" with the IP address of the Mac, for example as listed by `ifconfig vmnet8 | grep 'inet '`. If needed, replace `5555` with the other port you like from step 4\. (Note: one way to get command-line access to `adb` is via [**Tools > Android > Android Adb Command Prompt**](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat) in Visual Studio.)

### Alternate technique using `ssh`

If you have enabled _Remote Login_ on the Mac, then you can use `ssh` port forwarding to connect to the emulator.

1. Install an SSH client on Windows. One option is to install
    [Git for Windows](https://git-for-windows.github.io/). The `ssh`
    command will then be available in the **Git Bash** command prompt.

2. Follow steps 1-3 from above to start the emulator, kill the
    `adb` server on the Mac, and identify the emulator ports.

3. Run `ssh` on Windows to set up two-way port forwarding between a
    local port on Windows (`localhost:15555` in this example) and the
    odd-numbered emulator port on the Mac's loopback interface
    (`127.0.0.1:5555` in this example):

    ```cmd
    C:\> ssh -L localhost:15555:127.0.0.1:5555 mac-username@ip-address-of-the-mac
    ```

    Replace `mac-username` with your Mac username as listed by
    `whoami`. Replace `ip-address-of-the-mac` with the IP address of
    the Mac.

4. Connect to the emulator using the local port on Windows:

    ```cmd
    C:\> adb connect localhost:15555
    ```

    (Note: one easy way to get command-line access to `adb` is via
    [**Tools > Android > Android Adb Command Prompt** in Visual Studio](~/cross-platform/troubleshooting/questions/version-logs.md#adb-logcat).)

A small caution: if you use port `5555` for the local port, `adb` will
think that the emulator is running locally on Windows. This doesn't
cause any trouble in Visual Studio, but in Visual Studio for Mac it
causes the app to exit immediately after launch.

### Alternate technique using `adb -H` is not yet supported

In theory, another approach would be to use `adb`'s built-in capability
to connect to an `adb` server running on a remote machine (see for
example [https://stackoverflow.com/a/18551325](https://stackoverflow.com/a/18551325)).
But the Xamarin.Android IDE extensions do not currently provide a way
to configure that option.

## Contact information

This document discusses the current behavior as of March, 2016. The
technique described in this document is not part of the stable testing
suite for Xamarin, so it could break in the future.

If you notice that the technique no longer works, or if you notice any
other mistakes in the document, feel free to add to the discussion on
the following forum thread:
[http://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm](https://forums.xamarin.com/discussion/33702/android-emulator-from-host-device-inside-windows-vm).
Thanks!
