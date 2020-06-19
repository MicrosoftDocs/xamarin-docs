---
title: "Manually Signing the APK"
ms.prod: xamarin
ms.assetid: 08549E1C-7F04-4D20-9E7A-794B9D09FD12
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
---

# Manually Signing the APK

After the application has been built for release, the APK must be signed prior to distribution so that it can be run on an Android device. This process is typically handled with the IDE, however there are some situations where it is necessary to sign the APK manually, at the command line. The following steps are involved with signing an APK:

1. **Create a Private Key** &ndash; This step needs to be performed
    only once. A private key is necessary to digitally sign the APK.
    After the private key has been prepared, this step can be skipped
    for future release builds.

2. **Zipalign the APK** &ndash; *Zipalign* is an optimization process
    that is performed on an application. It enables Android to interact
    more efficiently with the APK at runtime. Xamarin.Android conducts
    a check at runtime, and will not allow the application to run if
    the APK has not been zipaligned.

3. **Sign the APK** &ndash; This step involves using the **apksigner** utility from the Android SDK and signing the APK with the private key that was created in the previous step. Applications that are developed with older versions of the Android SDK build tools prior to v24.0.3 will use the **jarsigner** app from the JDK. Both of these tools will be discussed in more detail below.

The order of the steps is important and is dependent on which tool used to sign the APK. When using **apksigner**, it is important to first **zipalign** the application, and then to sign it with **apksigner**.  If it is necessary to use **jarsigner** to sign the APK, then it is important to first sign the APK and then run **zipalign**.

## Prerequisites

This guide will focus on using **apksigner** from the Android SDK build
tools, v24.0.3 or higher. It assumes that an APK has already been
built.

Applications that are built using an older version of the Android SDK
Build Tools must use **jarsigner** as described in
[Sign the APK with jarsigner](#Sign_the_APK_with_jarsigner) below.

## Create a Private Keystore

A *keystore* is a database of security certificates that is created
by using the program
[keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)
from the Java SDK. A keystore is critical to publishing a
Xamarin.Android application, as Android will not run applications that
have not been digitally signed.

During development, Xamarin.Android uses a debug keystore to sign the
application, which allows the application to be deployed directly to
the emulator or to devices configured to use debuggable applications.
However, this keystore is not recognized as a valid keystore for the
purposes of distributing applications.

For this reason, a private keystore must be created and used for
signing applications. This is a step that should only be performed
once, as the same key will be used for publishing updates and can then
be used to sign other applications.

It is important to protect this keystore. If it is lost, then it will
not be possible to publish updates to the application with Google Play.
The only solution to the problem caused by a lost keystore would be to
create a new keystore, re-sign the APK with the new key, and then
submit a new application. Then the old application would have to be
removed from Google Play. Likewise, if this new keystore is compromised
or publicly distributed, then it is possible for unofficial or
malicious versions of an application to be distributed.

### Create a New Keystore

Creating a new keystore requires the command line tool
[keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)
from the Java SDK. The following snippet is an example of how to use
**keytool** (replace `<my-filename>` with the file name for the keystore
and `<key-name>` with the name of the key within the keystore):

```shell
$ keytool -genkeypair -v -keystore <filename>.keystore -alias <key-name> -keyalg RSA \
          -keysize 2048 -validity 10000
```

The first thing that **keytool** will ask for is the password for the
keystore. Then it will ask for some information to help with creating
the key. The following snippet is an example of creating a new key
called `publishingdoc` that will be stored in the file
`xample.keystore`:

```shell
$ keytool -genkeypair -v -keystore xample.keystore -alias publishingdoc -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Ham Chimpanze
What is the name of your organizational unit?
  [Unknown]:  NASA
What is the name of your organization?
  [Unknown]:  NASA
What is the name of your City or Locality?
  [Unknown]:  Cape Canaveral
What is the name of your State or Province?
  [Unknown]:  Florida
What is the two-letter country code for this unit?
  [Unknown]:  US
Is CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US
Enter key password for <publishingdoc>
        (RETURN if same as keystore password):
Re-enter new password:
[Storing xample.keystore]
```

To list the keys that are stored in a keystore, use the **keytool** with
the &ndash; `list` option:

```shell
$ keytool -list -keystore xample.keystore
```

## Zipalign the APK

Before signing an APK with **apksigner**, it is important to first optimize the file using the **zipalign** tool from the Android SDK. **zipalign** will restructure the resources in an APK along 4-byte boundaries. This alignment allows Android to quickly load the resources from the APK, increasing the performance of the application and potentially reducing memory use. Xamarin.Android will conduct a run-time check to determine if the APK has been zipaligned. If the APK is not zipaligned, then the application will not run.

The follow command will use the signed APK and produce a signed, zipaligned APK called **helloworld.apk** that is ready for distribution.

```shell
$ zipalign -f -v 4 mono.samples.helloworld-unsigned.apk helloworld.apk
```

## Sign the APK

After zipaligning the APK, it is necessary to sign it using a keystore. This is done with the **apksigner** tool, found in the **build-tools** directory of the version of the SDK build tools.  For example, if the Android SDK build tools v25.0.3 is installed, then **apksigner** can be found in the directory:

```bash
$ ls $ANDROID_HOME/build-tools/25.0.3/apksigner
/Users/tom/android-sdk-macosx/build-tools/25.0.3/apksigner*
```

The following snippet assumes that **apksigner** is accessible by the
`PATH` environment variable. It will sign an APK using the key alias
`publishingdoc` that is contained in the file **xample.keystore**:

```shell
$ apksigner sign --ks xample.keystore --ks-key-alias publishingdoc mono.samples.helloworld.apk
```

When this command is run, **apksigner** will ask for the password to the keystore if necessary.

See [Google's documentation](https://developer.android.com/studio/command-line/apksigner.html) for more details on the use of **apksigner**.

> [!NOTE]
> According to [Google issue 62696222](https://issuetracker.google.com/issues/62696222), **apksigner** is "missing" from the Android SDK. The workaround for this is to install the Android SDK build tools v25.0.3 and use that version of **apksigner**.  

<a name="Sign_the_APK_with_jarsigner"></a>

### Sign the APK with jarsigner

> [!WARNING]
> This section only applies if it is nececssary to sign the APK with the **jarsigner** utility. Developers are encouraged to use **apksigner** to sign the APK.

This technique involves signing the APK file using the **[jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)** command from the Java SDK.  The **jarsigner** tool is provided by the Java SDK.

The following shows how to sign an APK by using **jarsigner** and the key `publishingdoc` that is contained in a keystore file named **xample.keystore** :

```shell
$ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore xample.keystore mono.samples.helloworld.apk publishingdoc
```

> [!NOTE]
> When using **jarsigner**, it is important to sign the APK _first_, and then to use **zipalign**.  

## Related Links

- [Application Signing](https://source.android.com/security/apksigning/)
- [jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)
- [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)
- [zipalign](https://developer.android.com/studio/command-line/zipalign.html)
- [Build Tools 26.0.0 - where did apksigner go?](https://issuetracker.google.com/issues/62696222)
