---
title: "System.Exception AMDeviceNotificationSubscribe returned ..."
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 7E4ACC7E-F4FB-46C1-8837-C7FBAAFB2DC7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
---

# System.Exception AMDeviceNotificationSubscribe returned ...

> [!IMPORTANT]
> This issue has been resolved in recent versions of Xamarin. However, if the issue occurs on the latest version of the software, please file a [new bug](~/cross-platform/troubleshooting/questions/howto-file-bug.md) with your full versioning information and full build log output.


## Fix

1.  Kill the `usbmuxd` process so that the system will restart it:

    ```csharp
    sudo killall -QUIT usbmuxd
    ```

2.  If that doesn't solve the problem, reboot the Mac.

## Error message

```csharp
Exception: Exception type: System.Exception
AMDeviceNotificationSubscribe returned: 3892314211
  at Xamarin.MacDev.IPhoneDeviceManager.SubscribeToNotifications () [0x00000] in <filename unknown="">:0
  at Xamarin.MacDev.IPhoneDeviceManager.StartWatching (Boolean useOwnRunloop) [0x00000] in <filename unknown="">:0
  at Mtb.Server.DeviceListener.Start () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.RunDeviceListener () [0x00000] in <filename unknown="">:0
  at Mtb.Application.MainClass.Main (System.String[] args) [0x00000] in <filename unknown="">:0
```

This message can appear in an error dialog when you first start Visual Studio for Mac, or in the `mtbserver.log` file in the Xamarin.iOS Build Host app (**Xamarin.iOS Build Host > View Build Host Log**).

Note that this is an uncommon problem. If Visual Studio is having trouble connecting to the Mac build host, there are other errors that are more likely to appear in the `mtbserver.log` file.

### Errors in system.log

In some cases the following two error messages might also appear repeatedly in `/var/log/system.log`:

```csharp
17:17:11.369 usbmuxd[55040]: dnssd_clientstub ConnectToServer: socket failed 24 Too many open files
17:17:11.369 com.apple.usbmuxd[55040]: _BrowseReplyReceivedCallback Error doing DNSServiceResolve(): -65539
```

## Additional information

One guess at the root cause of the error is that the OS X system services responsible for reporting iOS device and simulator information can in rare cases enter an unexpected state. Xamarin cannot interact properly with the system services in this state. Rebooting the computer restarts the system services and solves the problem.

Based on the errors from `system.log` it appears that this problem might be related to Bonjour (`mDNSResponder`). Changing between different WiFi networks seems to increase the chances of hitting the problem.

## References

*   [Bug 11789 - MonoTouch.MobileDevice.MobileDeviceException: AMDeviceNotificationSubscribe returned: 0xe8000063 [RESOLVED NORESPONSE]](https://bugzilla.xamarin.com/show_bug.cgi?id=11789)
