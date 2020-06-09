---
title: "Fonts"
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/09/2018
---
# Fonts

## Overview

Beginning with API level 26, the Android SDK allows fonts to be treated as resources, just like a layouts or drawables. The [Android Support Library 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) will backport the new font API's to those apps that target API level 14 or higher.

After targeting API 26 or installing the Android Support Library v26, there are two ways to use fonts in an Android application:

1. **Package the font as an Android resource** &ndash; this ensures that the font is always available to the application, but will increase the size of the APK.
2. **Download the fonts** &ndash; Android also supports downloading a font from a _font provider_. The font provider checks if the font is already on the device. If necessary, the font will be downloaded and cached on the device. This font can be shared between multiple applications.

Similar fonts (or a font that may have several different styles) may be grouped into _font families_. This allows developers to specify certain attributes of the font, such as it's weight, and Android will automatically select the appropriate font from the font family.

The Android Support Library v26 will backport support for fonts to API level 26. When targeting the older API levels, it is necessary to declare the `app` XML namespace and to name the various font attributes using the  `android:` namespace and the `app:` namespace. If only the `android:` namespace is used, then the fonts will not be displayed devices running API level 25 or less. For example, this XML snippet declares a new [_font family_](#font_families) resource that will work in API level 14 and higher:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

     <font  android:font="@font/sourcesanspro_regular"
            android:fontStyle="normal"
            android:fontWeight="400"
            app:font="@font/sourcesanspro_regular"
            app:fontStyle="normal"
            app:fontWeight="400" />

</font-family>
```

As long as fonts are provided to an Android application in a proper way, they can be applied to a UI widget by setting the [`fontFamily` attribute](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily). For example, the following snippet demonstrates how to display a font in a TextView:

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

This guide will first discuss how to use fonts as an Android resource, and then move on to discuss how to download fonts at runtime.

## Fonts as a Resource

Packaging a font into an Android APK ensures that it is always available to the application. A font file (either a .TTF or a .OTF file) is added to a Xamarin.Android application just like any other resource, by copying files to a subdirectory in the **Resources** folder of a Xamarin.Android project. Fonts resources are kept in a **font** sub-directory of the **Resources** folder of the project.

> [!NOTE]
> The fonts should have a **Build Action** of **AndroidResource** or they will not be packaged into the final APK. The build action should be automatically set by the IDE.

When there are many similar font files (for example, the same font with different weights or styles) it is possible to group them into a font family.

<a name="font_families"></a>

### Font Families

A font family is a set of fonts that have different weights and styles. For example, there might be separate font files for bold or italic fonts. The font family is defined by `font` elements in an XML file that is kept in the  **Resources/font** directory. Each font family should have it's own XML file.

To create a font family, first add all the fonts to the **Resources/font** folder. Then create a new XML file in the font folder for the font family. The name of the XML file has no affinity or relationship to the fonts being referenced; the resource file can be any legal Android resource file name. This XML file will have a root `font-family` element that contains one or more `font` elements. Each `font` element declares the attributes of a font.

The following XML is an example of a font family for the _Sources Sans Pro_ font that defines many different font weights. This is saved as file in the **Resources/font** folder named **sourcesanspro.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto">
    <font android:font="@font/sourcesanspro_regular"
          android:fontStyle="normal"
          android:fontWeight="400"
          app:font="@font/sourcesanspro_regular"
          app:fontStyle="normal"
          app:fontWeight="400" />
    <font android:font="@font/sourcesanspro_bold"
          android:fontStyle="normal"
          android:fontWeight="800"
          app:font="@font/sourcesanspro_bold"
          app:fontStyle="normal"
          app:fontWeight="800" />
    <font android:font="@font/sourcesanspro_italic"
          android:fontStyle="italic"
          android:fontWeight="400"
          app:font="@font/sourcesanspro_italic"
          app:fontStyle="italic"
          app:fontWeight="400" />
</font-family>
```

The `fontStyle` attribute has two possible values:

- **normal** &ndash; a normal font
- **italic** &ndash; an italic font

The `fontWeight` attribute corresponds to the CSS `font-weight` attribute and refers to the thickness of the font. This is a value in the range of 100 - 900. The following list describes the common font weight values and their name:

- **Thin** &ndash; 100
- **Extra Light** &ndash; 200
- **Light** &ndash; 300
- **Normal** &ndash; 400
- **Medium** &ndash; 500
- **Semi Bold** &ndash; 600
- **Bold** &ndash; 700
- **Extra Bold** &ndash; 800
- **Black** &ndash; 900

Once a font family has been defined, it can be used declaratively by setting the `fontFamily`, `textStyle`, and `fontWeight` attributes in the layout file.  For example the following XML snippet sets a 600 weight font (normal) and an italic text style:

```xml
<TextView
    android:text="Sans Source Pro semi-bold italic, 600 weight, italic"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:fontFamily="@font/sourcesanspro"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_horizontal"
    android:fontWeight="600"
    android:textStyle="italic"
    />
```

### Programmatically Assigning Fonts

Fonts can be programmatically set by using the [`Resources.GetFont`](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) method to retrieve a [`Typeface`](https://developer.android.com/reference/android/graphics/Typeface.html) object. Many views have a `TypeFace` property that can be used to assign the font to the widget. This code snippet shows how to programmatically set the font on a TextView:

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

The `GetFont` method will automatically load the first font within a font family.  To load a font that matches a specific style, use the `Typeface.Create` method. This method will try to load a font that matches the specified style. As an example, this snippet will try to load a bold `Typeface` object from a font family that is defined in **Resources/fonts**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## Downloading Fonts

Instead of packaging fonts as an application resource, Android can download fonts from a remote source. This will have the desirable effect of reducing the size of the APK.

Fonts are downloaded with the assistance of a  _font provider_. This is a specialized content provider that manages the downloading and caching of fonts to all applications on the device. Android 8.0 includes a font provider to download fonts from the [Google Font Repository](https://fonts.google.com). This default font provider is backported to API level 14 with the Android Support Library v26.

When an app makes a request for a font, the font provider will first check to see if the font is already on the device. If not, it will then attempt to download the font. If the font cannot be downloaded, then Android will use the default system font. Once the font has been downloaded, it is available to all applications on the device, not just the app that made the initial request.

When a request is made to download a font, the app does not directly query the font provider. Instead, apps will use an instance of the [`FontsContract`](https://developer.android.com/reference/android/provider/FontsContract.html) API (or the [`FontsContractCompat`](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) if the Support Library 26 is being used).  

Android 8.0 supports downloading fonts in two different ways:

1. **Declare Downloadable Fonts as a Resource** &ndash; An app may declare downloadable fonts to Android via XML resource files. These files will contain all of the meta-data that Android needs to asynchronously download the fonts when the app starts and cache them on the device.
2. **Programmatically** &ndash; APIs in Android API level 26 allow an application to download the fonts programmatically, while the application is running. Apps will create a `FontRequest` object for a given font, and pass this object to the `FontsContract` class. The `FontsContract` takes the `FontRequest` and retrieves the font from a _font provider_. Android will synchronously download the font. An example of creating a `FontRequest` will be shown later in this guide.

Regardless of which approach is used, resources files that must be added to the Xamarin.Android application before fonts can be downloaded. First, the font(s) must be declared in an XML file in the **Resources/font** directory as part of a font family. This snippet is an example of how to download fonts from the [Google Fonts Open Source collection](https://fonts.google.com) using the default font provider that comes with Android 8.0 (or Support Library v26):

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto"
             android:fontProviderAuthority="com.google.android.gms.fonts"
             android:fontProviderPackage="com.google.android.gms"
             android:fontProviderQuery="VT323"
             android:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
             app:fontProviderAuthority="com.google.android.gms.fonts"
             app:fontProviderPackage="com.google.android.gms"
             app:fontProviderQuery="VT323"
             app:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
>
</font-family>
```

The `font-family` element contains the following attributes, declaring the information that Android requires to download the fonts:

1. **fontProviderAuthority** &ndash; The authority of the Font Provider to be used for the request.
2. **fontPackage** &ndash; The package for the Font Provider to be used for the request. This is used to verify the identity of the provider.
3. **fontQuery** &ndash; This is a string that will help the font provider locate the requested font. Details on the font query are specific to the font provider. The [`QueryBuilder`](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) class in the [Downloadable Fonts](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) sample app provides some information on the query format for fonts from the Google Fonts Open Source Collection.
4. **fontProviderCerts** &ndash;  A resource array with the list of sets of hashes for the certificates that the provider should be signed with.

Once the fonts are defined, it may be necessary to provide information about the _font certificates_ involved with the download.

### Font Certificates

If the font provider is not preinstalled on the device, or if the app is using the `Xamarin.Android.Support.Compat` library, Android requires the security certificates of the font provider. These certificates will be listed in an array resource file that is kept in **Resources/values** directory.

For example, the following XML is named **Resources/values/fonts_cert.xml** and stores the certificates for the Google font provider:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="com_google_android_gms_fonts_certs">
        <item>@array/com_google_android_gms_fonts_certs_dev</item>
        <item>@array/com_google_android_gms_fonts_certs_prod</item>
    </array>
    <string-array name="com_google_android_gms_fonts_certs_dev">
        <item>
            MIIEqDCCA5CgAwIBAgIJANWFuGx90071MA0GCSqGSIb3DQEBBAUAMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTAeFw0wODA0MTUyMzM2NTZaFw0zNTA5MDEyMzM2NTZaMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBANbOLggKv+IxTdGNs8/TGFy0PTP6DHThvbbR24kT9ixcOd9W+EaBPWW+wPPKQmsHxajtWjmQwWfna8mZuSeJS48LIgAZlKkpFeVyxW0qMBujb8X8ETrWy550NaFtI6t9+u7hZeTfHwqNvacKhp1RbE6dBRGWynwMVX8XW8N1+UjFaq6GCJukT4qmpN2afb8sCjUigq0GuMwYXrFVee74bQgLHWGJwPmvmLHC69EH6kWr22ijx4OKXlSIx2xT1AsSHee70w5iDBiK4aph27yH3TxkXy9V89TDdexAcKk/cVHYNnDBapcavl7y0RiQ4biu8ymM8Ga/nmzhRKya6G0cGw8CAQOjgfwwgfkwHQYDVR0OBBYEFI0cxb6VTEM8YYY6FbBMvAPyT+CyMIHJBgNVHSMEgcEwgb6AFI0cxb6VTEM8YYY6FbBMvAPyT+CyoYGapIGXMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbYIJANWFuGx90071MAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQEEBQADggEBABnTDPEF+3iSP0wNfdIjIz1AlnrPzgAIHVvXxunW7SBrDhEglQZBbKJEk5kT0mtKoOD1JMrSu1xuTKEBahWRbqHsXclaXjoBADb0kkjVEJu/Lh5hgYZnOjvlba8Ld7HCKePCVePoTJBdI4fvugnL8TsgK05aIskyY0hKI9L8KfqfGTl1lzOv2KoWD0KWwtAWPoGChZxmQ+nBli+gwYMzM1vAkP+aayLe0a1EQimlOalO762r0GXO0ks+UeXde2Z4e+8S/pf7pITEI/tP+MxJTALw9QUWEv9lKTk+jkbqxbsh8nfBUapfKqYn0eidpwq2AzVp3juYl7//fKnaPhJD9gs=
        </item>
    </string-array>
    <string-array name="com_google_android_gms_fonts_certs_prod">
        <item>
            MIIEQzCCAyugAwIBAgIJAMLgh0ZkSjCNMA0GCSqGSIb3DQEBBAUAMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDAeFw0wODA4MjEyMzEzMzRaFw0zNjAxMDcyMzEzMzRaMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBAKtWLgDYO6IIrgqWbxJOKdoR8qtW0I9Y4sypEwPpt1TTcvZApxsdyxMJZ2JORland2qSGT2y5b+3JKkedxiLDmpHpDsz2WCbdxgxRczfey5YZnTJ4VZbH0xqWVW/8lGmPav5xVwnIiJS6HXk+BVKZF+JcWjAsb/GEuq/eFdpuzSqeYTcfi6idkyugwfYwXFU1+5fZKUaRKYCwkkFQVfcAs1fXA5V+++FGfvjJ/CxURaSxaBvGdGDhfXE28LWuT9ozCl5xw4Yq5OGazvV24mZVSoOO0yZ31j7kYvtwYK6NeADwbSxDdJEqO4k//0zOHKrUiGYXtqw/A0LFFtqoZKFjnkCAQOjgdkwgdYwHQYDVR0OBBYEFMd9jMIhF1Ylmn/Tgt9r45jk14alMIGmBgNVHSMEgZ4wgZuAFMd9jMIhF1Ylmn/Tgt9r45jk14aloXikdjB0MQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEUMBIGA1UEChMLR29vZ2xlIEluYy4xEDAOBgNVBAsTB0FuZHJvaWQxEDAOBgNVBAMTB0FuZHJvaWSCCQDC4IdGZEowjTAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBBAUAA4IBAQBt0lLO74UwLDYKqs6Tm8/yzKkEu116FmH4rkaymUIE0P9KaMftGlMexFlaYjzmB2OxZyl6euNXEsQH8gjwyxCUKRJNexBiGcCEyj6z+a1fuHHvkiaai+KL8W1EyNmgjmyy8AW7P+LLlkR+ho5zEHatRbM/YAnqGcFh5iZBqpknHf1SKMXFh4dd239FJ1jWYfbMDMy3NS5CTMQ2XFI1MvcyUTdZPErjQfTbQe3aDQsQcafEQPD+nqActifKZ0Np0IS9L9kR/wbNvyz6ENwPiTrjV2KRkEjH78ZMcUQXg0L3BYHJ3lc69Vs5Ddf9uUGGMYldX3WfMBEmh/9iFBDAaTCK
        </item>
    </string-array>
</resources>
```

With these resource files in place, the app is capable of downloading the fonts.

### Declaring Downloadable Fonts as Resources

By listing the downloadable fonts in the **AndroidManifest.XML**, Android will asynchronously download the fonts when the app first starts. The font's themselves are listed in an array resource file, similar to this one:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

To download these fonts, they have to be declared in **AndroidManifest.XML** by adding `meta-data`  as a child of the `application` element. For example, if the downloadable fonts are declared in a resource file at **Resources/values/downloadable_fonts.xml**, then this snippet would have to be added to the manifest:

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### Downloading a Font with the Font APIs

It is possible to programmatically download a font by instantiating a [`FontRequest`](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) object and passing that to the  `FontContractCompat.RequestFont` method. The `FontContractCompat.RequestFont` method will first check to see if the font exists on the device, and then if necessary will asynchronously query the font provider and try to download the font for the app. If `FontRequest` is unable to download the font, then Android will use the default system font.

A `FontRequest` object contains information that will be used by the font provider to locate and download a font. A `FontRequest` requires four pieces of information:

1. **Font Provider Authority** &ndash; The authority of the Font Provider to be used for the request.
2. **Font Package** &ndash; The package for the Font Provider to be used for the request. This is used to verify the identity of the provider.
3. **Font Query** &ndash; This is a string that will help the font provider locate the requested font. Details on the font query are specific to the font provider. The details of the string are specific to the font provider. The [`QueryBuilder`](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) class in the [Downloadable Fonts](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) sample app provides some information on the query format for fonts from the Google Fonts Open Source Collection.
4. **Font Provider Certificates** &ndash;  A resource array with the list of sets of hashes for the certificates the provider should be signed with.

This snippet is an example of instantiating a new `FontRequest` object:

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

In the previous snippet `FontToDownload` is a query that will help the font from the Google Fonts Open Source collection.

Before passing the `FontRequest` to the `FontContractCompat.RequestFont` method, there are two objects that must be created:

- **`FontsContractCompat.FontRequestCallback`** &ndash; This is an abstract class which must be extended. It is a callback that will be invoked when `RequestFont` is finished. A Xamarin.Android app must subclass `FontsContractCompat.FontRequestCallback` and override the `OnTypefaceRequestFailed` and `OnTypefaceRetrieved`, providing the actions to be taken when the download fails or succeeds respectively.
- **`Handler`** &ndash; This is a `Handler` which will be used by `RequestFont` to download the font on a thread, if necessary. Fonts should **not** be downloaded on the UI thread.

This snippet is an example of a C# class that will asynchronously download a font from Google Fonts Open Source collection. It implements the `FontRequestCallback` interface, and raises a C# event when `FontRequest` has finished.

```csharp
public class FontDownloadHelper : FontsContractCompat.FontRequestCallback
{
    // A very simple font query; replace as necessary
    public static readonly String FontToDownload = "Courgette";

    Android.OS.Handler Handler = null;

    public event EventHandler<FontDownloadEventArg> FontDownloaded = delegate
    {
        // just an empty delegate to avoid null reference exceptions.  
    };

    public void DownloadFonts(Context context)
    {
        FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms",FontToDownload , Resource.Array.com_google_android_gms_fonts_certs);
        FontsContractCompat.RequestFont(context, request, this, GetHandlerThreadHandler());
    }

    public override void OnTypefaceRequestFailed(int reason)
    {
        base.OnTypefaceRequestFailed(reason);
        FontDownloaded(this, new FontDownloadEventArg(null));
    }

    public override void OnTypefaceRetrieved(Android.Graphics.Typeface typeface)
    {
        base.OnTypefaceRetrieved(typeface);
        FontDownloaded(this, new FontDownloadEventArg(typeface));
    }

    Handler GetHandlerThreadHandler()
    {
        if (Handler == null)
        {
            HandlerThread handlerThread = new HandlerThread("fonts");
            handlerThread.Start();
            Handler = new Handler(handlerThread.Looper);
        }
        return Handler;
    }
}

public class FontDownloadEventArg : EventArgs
{
    public FontDownloadEventArg(Android.Graphics.Typeface typeface)
    {
        Typeface = typeface;
    }
    public Android.Graphics.Typeface Typeface { get; private set; }
    public bool RequestFailed
    {
        get
        {
            return Typeface != null;
        }
    }
}
```

To use this helper, a new `FontDownloadHelper` is created, and an event handler is assigned:  

```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) =>
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

## Summary

This guide discussed the new APIs in Android 8.0 to support downloadable fonts and fonts as resources. It discussed how to embed existing fonts in an APK and to use them in a layout. It also discussed how Android 8.0 supports downloading fonts from a font provider, either programmatically or by declaring the font meta-data in resource files.

## Related Links

- [fontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [Typeface](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Android Support Library 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Using Fonts in Android](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [CSS font weight specification](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Google Fonts Open Source collection](https://fonts.google.com/)
- [Source Sans Pro](https://fonts.google.com/specimen/Source+Sans+Pro)
