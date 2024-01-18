---
title: "Working with the Android Manifest"
description: Learn about the AndroidManifest.xml file in the Android platform that allows you to describe the functionality and requirements of your application to Android.
ms.service: xamarin
ms.assetid: CB7CCF60-FEF1-3B28-215F-159391E74347
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/05/2018
---

# Working with the Android Manifest

**AndroidManifest.xml** is a powerful file in the Android platform that 
allows you to describe the functionality and requirements of your 
application to Android. However, working with it is not easy. 
Xamarin.Android helps to minimize this difficulty by allowing you to 
add custom attributes to your classes, which will then be used to 
automatically generate the manifest for you. Our goal is that 99% of 
our users should never need to manually modify **AndroidManifest.xml**. 

**AndroidManifest.xml** is generated as part of the build process, and 
the XML found within **Properties/AndroidManifest.xml** is merged with 
XML that is generated from custom attributes. The resulting merged 
**AndroidManifest.xml** resides in the **obj** subdirectory; for example, 
it resides at **obj/Debug/android/AndroidManifest.xml** for Debug builds. 
The merging process is trivial: it uses custom attributes within the 
code to generate XML elements, and *inserts* those elements into 
**AndroidManifest.xml**. 

## The Basics

At compile time, assemblies are scanned for non-`abstract` classes that derive from 
[Activity](xref:Android.App.Activity) 
and have the [`[Activity]`](xref:Android.App.ActivityAttribute) 
attribute declared on them. It then uses these classes and attributes 
to build the manifest. For example, consider the following code: 

```csharp
namespace Demo
{
    public class MyActivity : Activity
    {
    }
}
```

This results in nothing being generated in **AndroidManifest.xml**. If 
you want an `<activity/>` element to be generated, you need to use the 
[`[Activity]`](xref:Android.App.Activity) 
custom attribute: 

```csharp
namespace Demo
{
    [Activity]
    public class MyActivity : Activity
    {
    }
}
```

This example causes the following xml fragment to be added to **AndroidManifest.xml**:

```xml
<activity android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

The `[Activity]` attribute has no effect on `abstract`
types; `abstract` types are ignored.

### Activity Name

Beginning with Xamarin.Android 5.1, the type name of an activity is 
based on the MD5SUM of the assembly-qualified name of the type being 
exported. This allows the same fully-qualified name to be provided from 
two different assemblies and not get a packaging error. (Before 
Xamarin.Android 5.1, the default type name of the activity was created 
from the lowercased namespace and the class name.) 

If you wish to override this default and explicitly specify the name of your activity, use the 
[`Name`](xref:Android.App.ActivityAttribute.Name) property: 

```csharp
[Activity (Name="awesome.demo.activity")]
public class MyActivity : Activity
{
}
```

This example produces the following xml fragment:

```xml
<activity android:name="awesome.demo.activity" />
```

> [!NOTE]
> You should use the `Name` property only for 
> backward-compatibility reasons, as such renaming can slow down type 
> lookup at runtime. If you have legacy code that expects the default 
> type name of the activity to be based on the lowercased namespace and 
> the class name, see 
> [Android Callable Wrapper Naming](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#Android_Callable_Wrapper_Naming) 
> for tips on maintaining compatibility. 

### Activity Title Bar

By default, Android gives your application a title bar when it is run. 
The value used for this is 
[`/manifest/application/activity/@android:label`](https://developer.android.com/guide/topics/manifest/activity-element.html#label). 
In most cases, this value will differ from your class name. To specify your app's label on the title bar, 
use the [`Label`](xref:Android.App.ActivityAttribute.Label) property.
For example: 

```csharp
[Activity (Label="Awesome Demo App")]
public class MyActivity : Activity
{
}
```

This example produces the following xml fragment:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity" />
```

### Launchable from Application Chooser

By default, your activity will not show up in Android's application 
launcher screen. This is because there will likely be many activities 
in your application, and you don't want an icon for every one. To 
specify which one should be launchable from the application launcher, 
use the [`MainLauncher`](xref:Android.App.ActivityAttribute.MainLauncher) 
property. For example: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true)] 
public class MyActivity : Activity
{
}
```

This example produces the following xml fragment:

```xml
<activity android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

### Activity Icon

By default, your activity will be given the default launcher icon 
provided by the system. To use a custom icon, first add your **.png** to 
**Resources/drawable**, set its Build Action to **AndroidResource**, then 
use the [`Icon`](xref:Android.App.ActivityAttribute.Icon) 
property to specify the icon to use. For example: 

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
public class MyActivity : Activity
{
}
```

This example produces the following xml fragment:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
</activity>
```

### Permissions

When you add permissions to the Android Manifest (as described in 
[Add Permissions to Android Manifest](https://github.com/xamarin/recipes/tree/master/Recipes/android/general/projects/add_permissions_to_android_manifest)), 
these permissions are recorded in **Properties/AndroidManifest.xml**. 
For example, if you set the `INTERNET` permission, the following 
element is added to **Properties/AndroidManifest.xml**: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

Debug builds automatically set some permissions to make debug easier 
(such as `INTERNET` and `READ_EXTERNAL_STORAGE`) &ndash; these settings 
are set only in the generated **obj/Debug/android/AndroidManifest.xml** 
and are not shown as enabled in the **Required permissions** settings. 

For example, if you examine the generated manifest file at 
**obj/Debug/android/AndroidManifest.xml**, you may see the following 
added permission elements: 

```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
```

In the Release build version of the manifest (at 
**obj/Debug/android/AndroidManifest.xml**), these permissions are *not* 
automatically configured. If you find that switching to a Release build 
causes your app to lose a permission that was available in the Debug 
build, verify that you have explicitly set this permission in the 
**Required permissions** settings for your app (see **Build > Android 
Application** in Visual Studio for Mac; see **Properties > Android Manifest** 
in Visual Studio). 

## Advanced Features

### Intent Actions and Features

The Android manifest provides a way for you to describe the
capabilities of your activity. This is done via
[Intents](https://developer.android.com/guide/topics/manifest/intent-filter-element.html)
and the
[`[IntentFilter]`](xref:Android.App.IntentFilterAttribute)
custom attribute. You can specify which actions are appropriate for
your activity with the
[`IntentFilter`](xref:Android.App.IntentFilterAttribute#ctor*)
constructor, and which categories are appropriate with the
[`Categories`](xref:Android.App.IntentFilterAttribute.Categories)
property. At least one activity must be provided (which is why
activities are provided in the constructor). `[IntentFilter]` can be
provided multiple times, and each use results in a separate
`<intent-filter/>` element within the `<activity/>`. For example:

```csharp
[Activity (Label="Awesome Demo App", MainLauncher=true, Icon="@drawable/myicon")] 
[IntentFilter (new[]{Intent.ActionView}, 
        Categories=new[]{Intent.CategorySampleCode, "my.custom.category"})]
public class MyActivity : Activity
{
}
```

This example produces the following xml fragment:

```xml
<activity android:icon="@drawable/myicon" android:label="Awesome Demo App" 
          android:name="md5a7a3c803e481ad8926683588c7e9031b.MainActivity">
  <intent-filter>
    <action android:name="android.intent.action.MAIN" />
    <category android:name="android.intent.category.LAUNCHER" />
  </intent-filter>
  <intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.SAMPLE_CODE" />
    <category android:name="my.custom.category" />
  </intent-filter>
</activity>
```

### Application Element

The Android manifest also provides a way for you to declare properties 
for your entire application. This is done via the `<application>` 
element and its counterpart, the 
[Application](xref:Android.App.ApplicationAttribute) 
custom attribute. Note that these are application-wide (assembly-wide) 
settings rather than per-Activity settings. Typically, you declare 
`<application>` properties for your entire application and then 
override these settings (as needed) on a per-Activity basis. 

For example, the following `Application` attribute is added to 
**AssemblyInfo.cs** to indicate that the application can be debugged, 
that its user-readable name is **My App**, and that it uses the 
`Theme.Light` style as the default theme for all activities: 

```csharp
[assembly: Application (Debuggable=true,   
                        Label="My App",   
                        Theme="@android:style/Theme.Light")]
```

This declaration causes the following XML fragment to be generated in 
**obj/Debug/android/AndroidManifest.xml**:

```xml
<application android:label="My App" 
             android:debuggable="true" 
             android:theme="@android:style/Theme.Light"
                ... />
```

In this example, all activities in the app will default to the 
`Theme.Light` style. If you set an Activity's theme to 
`Theme.Dialog`, only that Activity will use the `Theme.Dialog` style 
while all other activities in your app will default to the 
`Theme.Light` style as set in the `<application>` element. 

The `Application` element is not the only way to configure 
`<application>` attributes. Alternately, you can insert attributes 
directly into the `<application>` element of 
**Properties/AndroidManifest.xml**. These settings are merged 
into the final `<application>` element that resides in 
**obj/Debug/android/AndroidManifest.xml**. Note that the contents of 
**Properties/AndroidManifest.xml** always override data provided by 
custom attributes. 

There are many application-wide attributes that you can configure in 
the `<application>` element; for more information about these settings, 
see the 
[Public Properties](xref:Android.App.ApplicationAttribute)
section of 
[ApplicationAttribute](xref:Android.App.ApplicationAttribute). 

## List of Custom Attributes

- [Android.App.ActivityAttribute](xref:Android.App.ActivityAttribute) : Generates a  [/manifest/application/activity](https://developer.android.com/guide/topics/manifest/activity-element.html) XML fragment 
- [Android.App.ApplicationAttribute](xref:Android.App.ApplicationAttribute) : Generates a  [/manifest/application](https://developer.android.com/guide/topics/manifest/application-element.html) XML fragment 
- [Android.App.InstrumentationAttribute](xref:Android.App.InstrumentationAttribute) : Generates a  [/manifest/instrumentation](https://developer.android.com/guide/topics/manifest/instrumentation-element.html) XML fragment 
- [Android.App.IntentFilterAttribute](xref:Android.App.IntentFilterAttribute) : Generates a  [//intent-filter](https://developer.android.com/guide/topics/manifest/intent-filter-element.html) XML fragment 
- [Android.App.MetaDataAttribute](xref:Android.App.MetaDataAttribute) : Generates a  [//meta-data](https://developer.android.com/guide/topics/manifest/meta-data-element.html) XML fragment 
- [Android.App.PermissionAttribute](xref:Android.App.PermissionAttribute) : Generates a  [//permission](https://developer.android.com/guide/topics/manifest/permission-element.html) XML fragment 
- [Android.App.PermissionGroupAttribute](xref:Android.App.PermissionGroupAttribute) : Generates a  [//permission-group](https://developer.android.com/guide/topics/manifest/permission-group-element.html) XML fragment 
- [Android.App.PermissionTreeAttribute](xref:Android.App.PermissionTreeAttribute) : Generates a  [//permission-tree](https://developer.android.com/guide/topics/manifest/permission-tree-element.html) XML fragment 
- [Android.App.ServiceAttribute](xref:Android.App.ServiceAttribute) : Generates a  [/manifest/application/service](https://developer.android.com/guide/topics/manifest/service-element.html) XML fragment 
- [Android.App.UsesLibraryAttribute](xref:Android.App.UsesLibraryAttribute) : Generates a  [/manifest/application/uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element.html) XML fragment 
- [Android.App.UsesPermissionAttribute](xref:Android.App.UsesPermissionAttribute) : Generates a  [/manifest/uses-permission](https://developer.android.com/guide/topics/manifest/uses-permission-element.html) XML fragment 
- [Android.Content.BroadcastReceiverAttribute](xref:Android.Content.BroadcastReceiverAttribute) : Generates a  [/manifest/application/receiver](https://developer.android.com/guide/topics/manifest/receiver-element.html) XML fragment 
- [Android.Content.ContentProviderAttribute](xref:Android.Content.ContentProviderAttribute) : Generates a  [/manifest/application/provider](https://developer.android.com/guide/topics/manifest/provider-element.html) XML fragment 
- [Android.Content.GrantUriPermissionAttribute](xref:Android.Content.GrantUriPermissionAttribute) : Generates a  [/manifest/application/provider/grant-uri-permission](https://developer.android.com/guide/topics/manifest/grant-uri-permission-element.html) XML fragment
