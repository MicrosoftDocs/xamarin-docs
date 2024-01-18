---
title: "Images in Xamarin.Forms"
description: "Images can be shared across platforms with Xamarin.Forms, they can be loaded specifically for each platform, or they can be downloaded for display."
ms.service: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Images in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/workingwithimages)

_Images can be shared across platforms with Xamarin.Forms, they can be loaded specifically for each platform, or they can be downloaded for display._

Images are a crucial part of application navigation, usability, and branding. Xamarin.Forms applications need to be able to share images across all platforms, but also potentially display different images on each platform.

Platform-specific images are also required for icons and splash screens; these need to be configured on a per-platform basis.

## Display images

Xamarin.Forms uses the [`Image`](xref:Xamarin.Forms.Image) view to display images on a page. It has several important properties:

- [`Source`](xref:Xamarin.Forms.Image.Source) - An [`ImageSource`](xref:Xamarin.Forms.ImageSource) instance, either File, Uri or Resource, which sets the image to display.
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect) - How to size the image within the bounds it is being displayed within (whether to stretch, crop or letterbox).

[`ImageSource`](xref:Xamarin.Forms.ImageSource) instances can be obtained using static methods for each type of image source:

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) - Requires a filename or filepath that can be resolved on each platform.
- [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) - Requires a Uri object, eg.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) - Requires a resource identifier to an image file embedded in the application or .NET Standard library project, with a **Build Action:EmbeddedResource**.
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) - Requires a stream that supplies image data.

The [`Aspect`](xref:Xamarin.Forms.Image.Aspect) property determines how the image will be scaled to fit the display area:

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) - Stretches the image to completely and exactly fill the display area. This may result in the image being distorted.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) - Clips the image so that it fills the display area while preserving the aspect (i.e. no distortion).
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) - Letterboxes the image (if required) so that the entire image fits into the display area, with blank space added to the top/bottom or sides depending on whether the image is wide or tall.

Images can be loaded from a [local file](#local-images), an [embedded resource](#embedded-images), [downloaded](#download-images), or loaded from a stream. In addition, font icons can be displayed by the [`Image`](xref:Xamarin.Forms.Image) view by specifying the font icon data in a `FontImageSource` object. For more information, see [Display font icons](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) in the [Fonts](~/xamarin-forms/user-interface/text/fonts.md) guide.

## Local images

Image files can be added to each application project and referenced from Xamarin.Forms shared code. This method of distributing images is required when images are platform-specific, such as when using different resolutions on different platforms, or slightly different designs.

To use a single image across all apps, *the same filename must be used on every platform*, and it should be a valid Android resource name (i.e. only lowercase letters, numerals, the underscore, and the period are allowed).

- **iOS** - The preferred way to manage and support images since iOS 9 is to use **Asset Catalog Image Sets**, which should contain all of the versions of an image that are necessary to support various devices and scale factors for an application. For more information, see [Adding Images to an Asset Catalog Image Set](~/ios/app-fundamentals/images-icons/displaying-an-image.md).
- **Android** - Place images in the  **Resources/drawable** directory with **Build Action: AndroidResource**. High- and low-DPI versions of an image can also be supplied (in appropriately named **Resources** subdirectories such as **drawable-ldpi**, **drawable-hdpi**, and **drawable-xhdpi**).
- **Universal Windows Platform (UWP)** - By default, images should be placed in the application's root directory with **Build Action: Content**. Alternatively, images can be placed in a different directory which is then specified with a platform-specific. For more information, see [Default image directory on Windows](~/xamarin-forms/platform/windows/default-image-directory.md).

> [!IMPORTANT]
> Prior to iOS 9, images were typically placed in the **Resources** folder with **Build Action: BundleResource**. However, this method of working with images in an iOS app has been deprecated by Apple. For more information, see [Image Sizes and Filenames](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Adhering to these rules for file naming and placement allows the following XAML to load and display the image on all platforms:

```xaml
<Image Source="waterfront.jpg" />
```

The equivalent C# code is as follows:

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

The following screenshots show the result of displaying a local image on each platform:

[![Sample application displaying a local image](images-images/local-sml.png)](images-images/local.png#lightbox)

For more flexibility the `Device.RuntimePlatform` property can be used to select a different image file or path for some or all platforms, as shown in this code example:

```csharp
image.Source = Device.RuntimePlatform == Device.Android
                ? ImageSource.FromFile("waterfront.jpg")
                : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> To use the same image filename across all platforms the name must be valid on all platforms. Android drawables have naming restrictions – only lowercase letters, numbers, underscore, and period are allowed – and for cross-platform compatibility this must be followed on all the other platforms too. The example filename **waterfront.png**
follows the rules, but examples of invalid filenames include "water front.png", "WaterFront.png", "water-front.png", and "wåterfront.png".

### Native resolutions (retina and high-DPI)

iOS, Android, and UWP include support for different image resolutions, where the operating system chooses the appropriate image at runtime based on the device's capabilities. Xamarin.Forms uses the native platforms' APIs for loading local images, so it automatically supports alternate resolutions if the files are correctly named and located in the project.

The preferred way to manage images since iOS 9 is to drag images for each resolution required to the appropriate asset catalog image set. For more information, see [Adding Images to an Asset Catalog Image Set](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Prior to iOS 9, retina versions of the image could be placed in the **Resources** folder - two and three times the resolution with a **@2x** or **@3x** suffixes on the filename before the file extension (eg. **myimage@2x.png**). However, this method of working with images in an iOS app has been deprecated by Apple. For more information, see [Image Sizes and Filenames](~/ios/app-fundamentals/images-icons/displaying-an-image.md).

Android alternate resolution images should be placed in [specially-named directories](https://developer.android.com/guide/practices/screens_support.html) in the Android project, as shown in the following screenshot:

[![Android multiple-resolution image location](images-images/xs-highdpisolution-sml.png)](images-images/xs-highdpisolution.png#lightbox)

UWP image file names [can be suffixed with `.scale-xxx` before the file extension](/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast), where `xxx` is the percentage of scaling applied to the asset, e.g. **myimage.scale-200.png**. Images can then be referred to in code or XAML without the scale modifier, e.g. just **myimage.png**. The platform will select the nearest appropriate asset scale based on the display's current DPI.

### Additional controls that display images

Some controls have properties that display an image, such as:

- [`Button`](xref:Xamarin.Forms.Button) has an [`ImageSource`](xref:Xamarin.Forms.Button.ImageSource) property that can be set to a bitmap image to be displayed on the `Button`. For more information, see [Using bitmaps with buttons](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons).
- [`ImageButton`](xref:Xamarin.Forms.Button) has a [`Source`](xref:Xamarin.Forms.ImageButton.Source) property that can be set to the image to display in the `ImageButton`. For more information, see [Setting the image source](~/xamarin-forms/user-interface/imagebutton.md#setting-the-image-source).
- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) has an [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) property that can be set to an image that's loaded from a file, embedded resource, URI, or stream.
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) has an [`ImageSource`](xref:Xamarin.Forms.ImageCell.ImageSource) property that can be set to an image retrieved from a file, embedded resource, URI, or stream.
- [`Page`](xref:Xamarin.Forms.Page). Any page type that derives from `Page` has [`IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) and [`BackgroundImageSource`](xref:Xamarin.Forms.Page.BackgroundImageSource) properties, which can be assigned a file, embedded resource, URI, or stream. Under certain circumstances, such as when a [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) is displaying a [`ContentPage`](xref:Xamarin.Forms.ContentPage), the icon will be displayed if supported by the platform.

  > [!IMPORTANT]
  > On iOS, the [`Page.IconImageSource`](xref:Xamarin.Forms.Page.IconImageSource) property can't be populated from an image in an asset catalog image set. Instead, load icon images for the `Page.IconImageSource` property from a file, embedded resource, URI, or stream.

## Embedded images

Embedded images are also shipped with an application (like local images) but instead of having a copy of the image in each application's file structure the image file is embedded in the assembly as a resource. This method of distributing images is recommended when identical images are used on each platform, and is particularly suited to creating components, as the image is bundled with the code.

To embed an image in a project, right-click to add new items and select the image/s you wish to add. By default the image will have **Build Action: None**; this needs to be set to **Build Action: EmbeddedResource**.

<!-- markdownlint-disable MD001 -->

# [Visual Studio](#tab/windows)

[![Set build action to embedded resource](images-images/vs-buildaction-sml.png)](images-images/vs-buildaction.png#lightbox)

The **Build Action** can be viewed and changed in the
**Properties** window for a file.

In this example the resource ID is **WorkingWithImages.beach.jpg**.
The IDE has generated this default by concatenating the **Default Namespace**
for this project with the filename, using a period (.) between each value.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# [Visual Studio for Mac](#tab/macos)

![Set Build Action: EmbeddedResource](images-images/xs-buildaction.png)

**Build Action** can also be viewed and changed in the
**Properties** pad for a file.
This pad shows the **Resource ID** that is used to
reference the resource in code. In the screenshot below, the **Resource ID**
is **WorkingWithImages.beach.jpg**.
The IDE has generated this default by concatenating the **Default Namespace**
for this project with the filename, using a period (.) between each value.
This ID can be edited in the **Properties** pad,
but for these examples the value **WorkingWithImages.beach.jpg** will be used.

[![Embedded resource properties pad](images-images/xs-embeddedproperties-sml.png)](images-images/xs-embeddedproperties.png#lightbox)

-----

If you place embedded images into folders within your project, the folder names are also separated by periods (.) in the resource ID. Moving the **beach.jpg** image into a folder called **MyImages** would result in a resource ID of **WorkingWithImages.MyImages.beach.jpg**

The code to load an embedded image simply passes the **Resource ID** to the [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) method as shown below:

```csharp
Image embeddedImage = new Image
{
    Source = ImageSource.FromResource("WorkingWithImages.beach.jpg", typeof(MyClass).GetTypeInfo().Assembly)
};
```

> [!NOTE]
> To support displaying embedded images in release mode on the Universal Windows Platform, it's necessary to use the overload of `ImageSource.FromResource` that specifies the source assembly in which to search for the image.

Currently there is no implicit conversion for resource identifiers. Instead, you must use [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) or `new ResourceImageSource()` to load embedded images.

The following screenshots show the result of displaying an embedded image on each platform:

[![Sample application displaying an embedded image](images-images/resource-sml.png)](images-images/resource.png#lightbox)

### XAML

Because there is no built-in type converter from `string` to `ResourceImageSource`, these types of images cannot be natively loaded by XAML. Instead, a simple custom XAML markup extension can be written to load images using a **Resource ID** specified in XAML:

```csharp
[ContentProperty (nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
 public string Source { get; set; }

 public object ProvideValue (IServiceProvider serviceProvider)
 {
   if (Source == null)
   {
     return null;
   }

   // Do your translation lookup here, using whatever method you require
   var imageSource = ImageSource.FromResource(Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);

   return imageSource;
 }
}
```

> [!NOTE]
> To support displaying embedded images in release mode on the Universal Windows Platform, it's necessary to use the overload of `ImageSource.FromResource` that specifies the source assembly in which to search for the image.

To use this extension add a custom `xmlns` to the XAML, using the correct namespace and assembly values for the project. The image source can then be set using this syntax: `{local:ImageResource WorkingWithImages.beach.jpg}`. A complete XAML example is shown below:

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage
   xmlns="http://xamarin.com/schemas/2014/forms"
   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
   xmlns:local="clr-namespace:WorkingWithImages;assembly=WorkingWithImages"
   x:Class="WorkingWithImages.EmbeddedImagesXaml">
 <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
   <!-- use a custom Markup Extension -->
   <Image Source="{local:ImageResource WorkingWithImages.beach.jpg}" />
 </StackLayout>
</ContentPage>
```

### Troubleshoot embedded images

#### Debug code

Because it is sometimes difficult to understand why a particular image resource isn't being loaded, the following debug code can be added temporarily to an application to help confirm the resources are correctly configured. It will output all known resources embedded in the given assembly to the **Console** to help debug resource loading issues.

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(MyClass).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### Images embedded in other projects

By default, the `ImageSource.FromResource` method only looks for images in the same assembly as the code calling the `ImageSource.FromResource` method. Using the debug code above you can determine which assemblies contain a specific resource by changing the `typeof()` statement to a `Type` known to be in each assembly.

However, the source assembly being searched for an embedded image can be specified as an argument to the `ImageSource.FromResource` method:

```csharp
var imageSource = ImageSource.FromResource("filename.png",
            typeof(MyClass).GetTypeInfo().Assembly);
```

## Download images

Images can be automatically downloaded for display, as shown in the following XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://aka.ms/campus.jpg" />
    <Label Text="campus.jpg gets downloaded from microsoft.com" />
  </StackLayout>
</ContentPage>
```

The equivalent C# code is as follows:

```csharp
var webImage = new Image {
     Source = ImageSource.FromUri(
        new Uri("https://aka.ms/campus.jpg")
     ) };
```

The [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) method requires a `Uri` object, and returns a new [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) that reads from the `Uri`.

There is also an implicit conversion for URI strings, so the following example will also work:

```csharp
webImage.Source = "https://aka.ms/campus.jpg";
```

The following screenshots show the result of displaying a remote image on each platform:

[![Sample application displaying a downloaded image](images-images/download-sml.png)](images-images/download.png#lightbox)

### Downloaded image caching

A [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) also supports caching of downloaded images, configured through the following properties:

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) - Whether caching is enabled (`true` by default).
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) - A `TimeSpan` that defines how long the image will be stored locally.

Caching is enabled by default and will store the image locally for 24 hours. To disable caching for a particular image, instantiate the image source as follows:

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri = new Uri("https://server.com/image") };
```

To set a specific cache period (for example, 5 days) instantiate the image source as follows:

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://aka.ms/campus.jpg"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

Built-in caching makes it very easy to support scenarios like scrolling lists of images, where you can set (or bind) an image in each cell and let the built-in cache take care of re-loading the image when the cell is scrolled back into view.

## Animated GIFs

Xamarin.Forms includes support for displaying small, animated GIFs. This is accomplished by setting the [`Image.Source`](xref:Xamarin.Forms.Image.Source) property to an animated GIF file:

```xaml
<Image Source="demo.gif" />
```

> [!IMPORTANT]
> While the animated GIF support in Xamarin.Forms includes the ability to download files, it does not support caching or streaming animated GIFs.

By default, when an animated GIF is loaded it will not be played. This is because the `IsAnimationPlaying` property, that controls whether an animated GIF is playing or stopped, has a default value of `false`. This property, of type `bool`, is backed by a [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) object, which means that it can be the target of a data binding, and styled.

Therefore, when an animated GIF is loaded it will not be played until the `IsAnimationPlaying` property is set to `true`. Playback can then be stopped by setting the `IsAnimationPlaying` property to `false`. Note that this property has no effect when displaying a non-GIF image source.

> [!NOTE]
> On Android, animated GIF support requires that your application is using fast renderers, and won't work if you've opted into using the legacy renderers.
> On UWP, animated GIF support requires a minimum release of Windows 10 Anniversary Update (version 1607).

## Icons and splash screens

While not related to the [`Image`](xref:Xamarin.Forms.Image) view, application icons and splash screens are also an important use of images in Xamarin.Forms projects.

Setting icons and splash screens for Xamarin.Forms apps is done in each of the application projects. This means generating correctly sized images for iOS, Android, and UWP. These images should be named and located according to each platforms' requirements.

## Icons

See the [iOS Working with Images](~/ios/app-fundamentals/images-icons/index.md), [Google Iconography](https://developer.android.com/design/style/iconography.html), and [UWP Guidelines for tile and icon assets](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) for more information on creating these application resources.

In addition, font icons can be displayed by the [`Image`](xref:Xamarin.Forms.Image) view by specifying the font icon data in a `FontImageSource` object. For more information, see [Display font icons](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) in the [Fonts](~/xamarin-forms/user-interface/text/fonts.md) guide.

## Splash screens

Only iOS and UWP applications require a splash screen (also called a startup screen or default image).

Refer to the documentation for [iOS Working with Images](~/ios/app-fundamentals/images-icons/index.md) and [Splash screens](/windows/uwp/launch-resume/splash-screens/) on the Windows Dev Center.

## Related links

- [WorkingWithImages (sample)](/samples/xamarin/xamarin-forms-samples/workingwithimages)
- [iOS Working with Images](~/ios/app-fundamentals/images-icons/index.md)
- [Android Iconography](https://developer.android.com/design/style/iconography.html)
- [Guidelines for tile and icon assets](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)