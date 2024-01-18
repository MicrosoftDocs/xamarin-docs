---
title: "Picking a Photo from the Picture Library"
description: "This article explains how to use the Xamarin.Forms DependencyService class to pick a photo from the phone's picture library."
ms.service: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Picking a Photo from the Picture Library

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/dependencyservice/)

This article walks through the creation of an application that allows the user to pick a photo from the phone's picture library. Because Xamarin.Forms does not include this functionality, it is necessary to use [`DependencyService`](xref:Xamarin.Forms.DependencyService) to access native APIs on each platform.

## Creating the interface

First, create an interface in shared code that expresses the desired functionality. In the case of a photo-picking application, just one method is required. This is defined in the  [`IPhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos/Services/IPhotoPickerService.cs) interface in the .NET Standard library of the sample code:

```csharp
namespace DependencyServiceDemos
{
    public interface IPhotoPickerService
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

The `GetImageStreamAsync` method is defined as asynchronous because the method must return quickly, but it can't return a `Stream` object for the selected photo until the user has browsed the picture library and selected one.

This interface is implemented in all the platforms using platform-specific code.

## iOS implementation

The iOS implementation of the `IPhotoPickerService` interface uses the [`UIImagePickerController`](xref:UIKit.UIImagePickerController) as described in the [**Choose a Photo from the Gallery**](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery) recipe and [sample code](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

The iOS implementation is contained in the [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.iOS/Services/PhotoPickerService.cs) class in the iOS project of the sample code. To make this class visible to the `DependencyService` manager, the class must be identified with an [`assembly`] attribute of type `Dependency`, and the class must be public and explicitly implement the `IPhotoPickerService` interface:

```csharp
[assembly: Dependency (typeof (PhotoPickerService))]
namespace DependencyServiceDemos.iOS
{
    public class PhotoPickerService : IPhotoPickerService
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;

        public Task<Stream> GetImageStreamAsync()
        {
            // Create and define UIImagePickerController
            imagePicker = new UIImagePickerController
            {
                SourceType = UIImagePickerControllerSourceType.PhotoLibrary,
                MediaTypes = UIImagePickerController.AvailableMediaTypes(UIImagePickerControllerSourceType.PhotoLibrary)
            };

            // Set event handlers
            imagePicker.FinishedPickingMedia += OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled += OnImagePickerCancelled;

            // Present UIImagePickerController;
            UIWindow window = UIApplication.SharedApplication.KeyWindow;
            var viewController = window.RootViewController;
            viewController.PresentViewController(imagePicker, true, null);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<Stream>();
            return taskCompletionSource.Task;
        }
        ...
    }
}

```

The `GetImageStreamAsync` method creates a `UIImagePickerController` and initializes it to select images from the photo library. Two event handlers are required: One for when the user selects a photo and the other for when the user cancels the display of the photo library. The `PresentViewController` method then displays the photo library to the user.

At this point, the `GetImageStreamAsync` method must return a `Task<Stream>` object to the code that's calling it. This task is completed only when the user has finished interacting with the photo library and one of the event handlers is called. For situations like this, the [`TaskCompletionSource`](/dotnet/api/system.threading.tasks.taskcompletionsource-1) class is essential. The class provides a `Task` object of the proper generic type to return from the `GetImageStreamAsync` method, and the class can later be signaled when the task is completed.

The `FinishedPickingMedia` event handler is called when the user has selected a picture. However, the handler provides a `UIImage` object and the `Task` must return a .NET `Stream` object. This is done in two steps: The `UIImage` object is first converted to an in memory PNG or JPEG file stored in an `NSData` object, and then the `NSData` object is converted to a .NET `Stream` object. A call to the `SetResult` method of the `TaskCompletionSource` object completes the task by providing the `Stream` object:

```csharp
namespace DependencyServiceDemos.iOS
{
    public class PhotoPickerService : IPhotoPickerService
    {
        TaskCompletionSource<Stream> taskCompletionSource;
        UIImagePickerController imagePicker;
        ...
        void OnImagePickerFinishedPickingMedia(object sender, UIImagePickerMediaPickedEventArgs args)
        {
            UIImage image = args.EditedImage ?? args.OriginalImage;

            if (image != null)
            {
                // Convert UIImage to .NET Stream object
                NSData data;
                if (args.ReferenceUrl.PathExtension.Equals("PNG") || args.ReferenceUrl.PathExtension.Equals("png"))
                {
                    data = image.AsPNG();
                }
                else
                {
                    data = image.AsJPEG(1);
                }
                Stream stream = data.AsStream();

                UnregisterEventHandlers();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
            }
            else
            {
                UnregisterEventHandlers();
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            UnregisterEventHandlers();
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }

        void UnregisterEventHandlers()
        {
            imagePicker.FinishedPickingMedia -= OnImagePickerFinishedPickingMedia;
            imagePicker.Canceled -= OnImagePickerCancelled;
        }
    }
}
```

An iOS application requires permission from the user to access the phone's photo library. Add the following to the `dict` section of the Info.plist file:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```

## Android implementation

The Android implementation uses the technique described in the [**Select an Image**](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image) recipe and the [sample code](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image). However, the method that is called when the user has selected an image from the picture library is an `OnActivityResult` override in a class that derives from `Activity`. For this reason, the normal [`MainActivity`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.Android/MainActivity.cs) class in the Android project has been supplemented with a field, a property, and an override of the `OnActivityResult` method:

```csharp
public class MainActivity : FormsAppCompatActivity
{
    internal static MainActivity Instance { get; private set; }  

    protected override void OnCreate(Bundle savedInstanceState)
    {
        // ...
        Instance = this;
    }
    // ...
    // Field, property, and method for Picture Picker
    public static readonly int PickImageId = 1000;

    public TaskCompletionSource<Stream> PickImageTaskCompletionSource { set; get; }

    protected override void OnActivityResult(int requestCode, Result resultCode, Intent intent)
    {
        base.OnActivityResult(requestCode, resultCode, intent);

        if (requestCode == PickImageId)
        {
            if ((resultCode == Result.Ok) && (intent != null))
            {
                Android.Net.Uri uri = intent.Data;
                Stream stream = ContentResolver.OpenInputStream(uri);

                // Set the Stream as the completion of the Task
                PickImageTaskCompletionSource.SetResult(stream);
            }
            else
            {
                PickImageTaskCompletionSource.SetResult(null);
            }
        }
    }
}
```

The `OnActivityResult`override indicates the selected picture file with an Android `Uri` object, but this can be converted into a .NET `Stream` object by calling the `OpenInputStream` method of the `ContentResolver` object that was obtained from the activity's `ContentResolver` property.

Like the iOS implementation, the Android implementation uses a `TaskCompletionSource` to signal when the task has been completed. This `TaskCompletionSource` object is defined as a public property in the `MainActivity` class. This allows the property to be referenced in the [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.Android/Services/PhotoPickerService.cs) class in the Android project. This is the class with the `GetImageStreamAsync` method:

```csharp
[assembly: Dependency(typeof(PhotoPickerService))]
namespace DependencyServiceDemos.Droid
{
    public class PhotoPickerService : IPhotoPickerService
    {
        public Task<Stream> GetImageStreamAsync()
        {
            // Define the Intent for getting images
            Intent intent = new Intent();
            intent.SetType("image/*");
            intent.SetAction(Intent.ActionGetContent);

            // Start the picture-picker activity (resumes in MainActivity.cs)
            MainActivity.Instance.StartActivityForResult(
                Intent.CreateChooser(intent, "Select Picture"),
                MainActivity.PickImageId);

            // Save the TaskCompletionSource object as a MainActivity property
            MainActivity.Instance.PickImageTaskCompletionSource = new TaskCompletionSource<Stream>();

            // Return Task object
            return MainActivity.Instance.PickImageTaskCompletionSource.Task;
        }
    }
}
```

This method accesses the `MainActivity` class for several purposes: for the `Instance` property, for the `PickImageId` field, for the `TaskCompletionSource` property, and to call `StartActivityForResult`. This method is defined by the `FormsAppCompatActivity` class, which is the base class of `MainActivity`.

## UWP implementation

Unlike the iOS and Android implementations, the implementation of the photo picker for the Universal Windows Platform does not require the `TaskCompletionSource` class. The [`PhotoPickerService`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceDemos.UWP/Services/PhotoPickerService.cs) class uses the [`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) class to get access to the photo library. Because the `PickSingleFileAsync` method of `FileOpenPicker` is itself asynchronous, the `GetImageStreamAsync` method can simply use `await` with that method (and other asynchronous methods) and return a `Stream` object:

```csharp
[assembly: Dependency(typeof(PhotoPickerService))]
namespace DependencyServiceDemos.UWP
{
    public class PhotoPickerService : IPhotoPickerService
    {
        public async Task<Stream> GetImageStreamAsync()
        {
            // Create and initialize the FileOpenPicker
            FileOpenPicker openPicker = new FileOpenPicker
            {
                ViewMode = PickerViewMode.Thumbnail,
                SuggestedStartLocation = PickerLocationId.PicturesLibrary,
            };

            openPicker.FileTypeFilter.Add(".jpg");
            openPicker.FileTypeFilter.Add(".jpeg");
            openPicker.FileTypeFilter.Add(".png");

            // Get a file and return a Stream
            StorageFile storageFile = await openPicker.PickSingleFileAsync();

            if (storageFile == null)
            {
                return null;
            }

            IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
            return raStream.AsStreamForRead();
        }
    }
}
```

## Implementing in shared code

Now that the interface has been implemented for each platform, the shared code in the .NET Standard library can take advantage of it.

The UI includes a [`Button`](xref:Xamarin.Forms.Button) that can be clicked to choose a photo:

```xaml
<Button Text="Pick Photo"
        Clicked="OnPickPhotoButtonClicked" />
```

The `Clicked` event handler uses the `DependencyService` class to call `GetImageStreamAsync`. This results in a call to the platform project. If the method returns a `Stream` object, then the handler sets the `Source` property of the `image` object to the `Stream` data:

```csharp
async void OnPickPhotoButtonClicked(object sender, EventArgs e)
{
    (sender as Button).IsEnabled = false;

    Stream stream = await DependencyService.Get<IPhotoPickerService>().GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }

    (sender as Button).IsEnabled = true;
}
```

## Related links

- [DependencyService (sample)](/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [Choose a Photo from the Gallery (iOS)](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery)
- [Select an Image (Android)](https://github.com/xamarin/recipes/tree/master/Recipes/android/other_ux/pick_image)