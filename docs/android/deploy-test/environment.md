---
title: "Xamarin.Android Environment"
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
---

# Xamarin.Android Environment

## Execution Environment

The *execution environment* is the set of environment variables and
Android system properties that influence program execution. Android
system properties can be set with the `adb shell setprop` command,
while environment variables can be set by setting the `debug.mono.env`
system property:

```shell
## Enable GREF logging
adb shell setprop debug.mono.log gref

## Set the MONO_LOG_LEVEL and MONO_LOG_MASK environment variables
## so that additional Mono messages will be written to `adb logcat`.
adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
```

Android system properties are set for all processes on the target
device.

Starting with Xamarin.Android 4.6, both system properties and
environment variables may be set or overridden on a per-app basis by
adding an *environment file* to the project. An environment file is a
Unix-formatted plain-text file with a
[**Build action** of `AndroidEnvironment`](~/android/deploy-test/building-apps/build-process.md).
The environment file contains lines with the format *key=value*.
Comments are lines which start with `#`. Blank lines are ignored.

If *key* starts with an uppercase letter, then *key* is treated as an
environment variable and **setenv**(3) is used to set the environment
variable to the specified *value* during process startup.

If *key* starts with a lowercase letter, then *key* is treated as an
Android system property and *value* is the *default value*: Android
system properties which control Xamarin.Android execution behavior are
looked up first from the Android system property store, and if no
value is present then the value specified in the environment file is
used. This is to permit `adb shell setprop` to be used to override
values which come from the environment file for diagnostic purposes.

## Xamarin.Android Environment Variables

Xamarin.Android supports the `XA_HTTP_CLIENT_HANDLER_TYPE` variable,
which may be set either via `adb shell setprop debug.mono.env` or via
the `$(AndroidEnvironment)` Build action.

### `XA_HTTP_CLIENT_HANDLER_TYPE`

The assembly-qualified type which must inherit from
[HttpMessageHandler](/dotnet/api/system.net.http.httpmessagehandler?view=xamarinandroid-7.1)
and is constructed from the
[`HttpClient()` default constructor](/dotnet/api/system.net.http.httpclient.-ctor?view=xamarinandroid-7.1#System_Net_Http_HttpClient__ctor).

In Xamarin.Android 6.1, this environment variable is not set by
default, and
[HttpClientHandler](/dotnet/api/system.net.http.httpclienthandler?view=xamarinandroid-7.1)
will be used.

Alternatively, the value `Xamarin.Android.Net.AndroidClientHandler` may
be specified to use
[`java.net.URLConnection`](xref:Java.Net.URLConnection)
for network access, which *may* permit use of TLS 1.2 when Android
supports it.

Added in Xamarin.Android 6.1.

## Xamarin.Android System Properties

Xamarin.Android supports the following system properties, which may be
set either via `adb shell setprop` or via the `$(AndroidEnvironment)`
Build action.

- `debug.mono.debug`
- `debug.mono.env`
- `debug.mono.gc`
- `debug.mono.log`
- `debug.mono.max_grefc`
- `debug.mono.profile`
- `debug.mono.runtime_args`
- `debug.mono.trace`
- `debug.mono.wref`
- `XA_HTTP_CLIENT_HANDLER_TYPE`

### `debug.mono.debug`

The value of the `debug.mono.debug` system property is an integer. If
`1`, then behave "as if" the process were started with `mono --debug`.
This generally shows file and line information in stack traces, etc.,
without requiring that the app be started from a debugger.

### `debug.mono.env`

Contains a `|`-separated list of environment variables.

### `debug.mono.gc`

The value of the `debug.mono.gc` system property is an integer.
If `1`, then GC information should be logged.

This is equivalent to having the `debug.mono.log` system property contain `gc`.

### `debug.mono.log`

Controls which additional information Xamarin.Android will log to `adb logcat`.
It is a comma-separated string (`,`), containing one of the following values:

- `all`: Print out *all* messages. This is seldom a good idea, as it includes
  `lref` messages.
- `assembly`: Print out `.apk` and assembly parsing messages.
- `gc`: Print out GC-related messages.
- `gref`: Print out JNI Global Reference messages.
- `lref`: Print out JNI Local Reference messages.
  > [!NOTE]
  > This will *really* spam `adb logcat`.
  > In Xamarin.Android 5.1, this will also create a `.__override__/lrefs.txt`
  > file, which can get *gigantic*.
  > Avoid.
- `timing`: Print out some method timing information. This will also create
  the files `.__override__/methods.txt` and `.__override__/counters.txt`.

### `debug.mono.max_grefc`

The value of the `debug.mono.max_grefc` system property is an integer.
It's value *overrides* the default detected maximum GREF count for the
target device.

*Please note:* This is only usable with `adb shell setprop
debug.mono.max_grefc` as the value will not be available in time with
an **environment.txt** file.

### `debug.mono.profile`

The `debug.mono.profile` system property enables the profiler.
It is equivalent to, and uses the same values as, the `mono --profile`
option. (See the [**mono**(1)](http://docs.go-mono.com/?link=man%3amono(1))
man page for more information.)

### `debug.mono.runtime_args`

The `debug.mono.runtime_args` system property contains additional
options that should be parsed by **mono**.

### `debug.mono.trace`

The `debug.mono.trace` system property enables tracing.
It is equivalent to, and uses the same values as, the `mono --trace`
option. (See the [**mono**(1)](http://docs.go-mono.com/?link=man%3amono(1))
man page for more information.)

In general, *do not use*. Use of tracing will spam `adb logcat` output,
severaly slow down program behavior, and alter program behavior (up to
and including adding additional error conditions).

*Sometimes*, however, it allows some additional investigation to be performed...

### `debug.mono.wref`

The `debug.mono.wref` system property allows overriding the default detected
JNI Weak Reference mechanism. There are two supported values:

- `jni`: Use JNI weak references, as created by `JNIEnv::NewWeakGlobalRef()`
  and destroyed by `JNIEnv::DeleteWeakGlobalREf()`.
- `java`: Use JNI Global references which reference
  `java.lang.WeakReference` instances.

`java` is used, by default, up through API-7 and on API-19 (Kit Kat) with
ART enabled. (API-8 added `jni` references, and ART *broke* `jni` references.)

This system property is useful for testing and certain forms of investigation.
*In general*, it should not be changed.

### XA\_HTTP\_CLIENT\_HANDLER\_TYPE

First introduced in Xamarin.Android 6.1, this environment variable
declares the default `HttpMessageHandler` implementation that will be
used by the `HttpClient`. By default this variable is not set, and
Xamarin.Android will use the `HttpClientHandler`.

```shell
XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
```

> [!NOTE]
> The underlying Android device must support TLS 1.2.
Android 5.0 and later support TLS 1.2

## Example

```shell
## Comments are lines which start with '#'
## Blank lines are ignored.

## Enable GREF messages to `adb logcat`
debug.mono.log=gref

## Clear out a Mono environment variable to decrease logging
MONO_LOG_LEVEL=
```

## Related Links

- [Transport Layer Security](~/cross-platform/app-fundamentals/transport-layer-security.md)
