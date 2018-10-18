---
title: "Finding your Keystore's Signature"
ms.prod: xamarin
ms.assetid: 1b511fec-e6f6-453e-89c8-810aafb02b77
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
---

# Finding your Keystore's Signature

The MD5 or SHA1 signature of a Xamarin.Android app depends on the
**.keystore** file that was used to sign the APK. Typically, a debug
build will use a different **.keystore** file than a release build.

## For Debug / Non-Custom Signed Builds

Xamarin.Android signs all debug builds with the same **debug.keystore**
file. This file is generated when Xamarin.Android is first
installed.The steps below detail the process for finding the MD5 or
SHA1 signature of the default Xamarin.Android **debug.keystore** file.

# [Visual Studio](#tab/windows)

Locate the Xamarin **debug.keystore** file that is used to sign the
app. By default, the keystore that is used to sign debug versions of
a Xamarin.Android application can be found at the following
location:

**C:\\Users\\*USERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\debug.keystore**

Information about a keystore is obtained by running the `keytool.exe`
command from the JDK. This tool is typically found in the following location:

**C:\\Program Files (x86)\\Java\\jdk*VERSION*\\bin\\keytool.exe**

Add the directory containing **keytool.exe** to the `PATH` environment variable.
Open a **Command Prompt** and run `keytool.exe` using the following command:

```cmd
keytool.exe -list -v -keystore "%LocalAppData%\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

When run, **keytool.exe** should output the following text. The **MD5:** and **SHA1:** labels identify the respective signatures:

```cmd
Alias name: androiddebugkey
Creation date: Aug 19, 2014
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 53f3b126
Valid from: Tue Aug 19 13:18:46 PDT 2014 until: Sun Nov 15 12:18:46 PST 2043
Certificate fingerprints:
         MD5:  27:78:7C:31:64:C2:79:C6:ED:E5:80:51:33:9C:03:57
         SHA1: 00:E5:8B:DA:29:49:9D:FC:1D:DA:E7:EE:EE:1A:8A:C7:85:E7:31:23
         SHA256: 21:0D:73:90:1D:D6:3D:AB:4C:80:4E:C4:A9:CB:97:FF:34:DD:B4:42:FC:
08:13:E0:49:51:65:A6:7C:7C:90:45
         Signature algorithm name: SHA1withRSA
         Version: 3
```


# [Visual Studio for Mac](#tab/macos)

Locate the Xamarin **debug.keystore** file that is used to sign the
app. By default, the keystore that is used to sign debug versions of
a Xamarin.Android application can be found at the following
location:

**~/.local/share/Xamarin/Mono for Android/debug.keystore**


Information about a keystore is obtained by running the **keytool**
command from the JDK. This tool is typically found in the following
location:

**/System/Library/Java/JavaVirtualMachines/*VERSION*.jdk/Contents/Home/bin/keytool**

Add the directory containing **keytool** to the **PATH** environment variable.
Open a **Terminal** and run **keytool**
by using the following command:

```bash
$ keytool -list -v -keystore ~/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

When run, **keytool** should output the following text. The **MD5:** and **SHA1:** labels identify the respective signatures:

```bash
Alias name: androiddebugkey
Creation date: Apr 16, 2015
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 76afa120
Valid from: Thu Apr 16 10:52:27 PDT 2015 until: Sat Apr 08 10:52:27 PDT 2045
Certificate fingerprints:
     MD5:  0A:D3:7E:80:3D:40:2A:23:89:B9:AB:9C:4B:B6:63:36
     SHA1: 89:33:8F:F2:C5:0C:91:08:4A:CF:04:A5:EC:4A:31:80:84:18:0D:D4
     SHA256: 91:AC:3E:2F:CB:EF:50:07:2B:E0:D9:8D:8B:C2:42:87:6A:85:02:86:EB:44:84:10:34:02:ED:35:CE:C6:38:47
     Signature algorithm name: SHA256withRSA
     Version: 3

Extensions:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 65 6C FE 67 8E CD CB 9A   01 CD 9F E8 25 9C A4 A3  el.g........%...
0010: 4F 4E CF 17                                        ON..
]
]
```

-----

## For Release / Custom Signed Builds

The process for release builds that are signed with a custom
**.keystore** file are the same as above, with the release
**.keystore** file replacing the **debug.keystore** file that is used
by Xamarin.Android. Replace your own values for the keystore password,
and alias name from when the release keystore file was created.

# [Visual Studio](#tab/windows)

When the Visual Studio **Distribute**
wizard is used to sign a Xamarin.Android app, the resulting keystore resides in the following location:

**C:\\Users\\*USERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\Keystore\\*alias*\\*alias*.keystore**

For example, if you followed the steps in [Create a New Certificate](~/android/deploy-test/signing/index.md#newcertvs) to create a new signing key, the resulting example keystore resides in the following location:

**C:\\Users\\*USERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\Keystore\\chimp\\chimp.keystore**

For more information about signing a Xamarin.Android app, see
[Signing the Android Application Package](~/android/deploy-test/signing/index.md).


# [Visual Studio for Mac](#tab/macos)

When the Visual Studio for Mac **Sign and Distribute...**
wizard to sign your app, the resulting keystore resides in the following location:

**~/Library/Developer/Xamarin/Keystore/*alias*/*alias*.keystore**

For example, if you followed the steps in [Create a New Certificate](~/android/deploy-test/signing/index.md#newcertxs) to create a new signing key, the resulting example keystore resides in the following location:

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**

For more information about signing a Xamarin.Android app, see
[Signing the Android Application Package](~/android/deploy-test/signing/index.md).


-----
