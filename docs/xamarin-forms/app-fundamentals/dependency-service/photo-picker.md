---
title: "Picking a Photo from the Picture Library"
description: "Use DependencyService to pick a photo from the phone's picture library"
ms.prod: xamarin
ms.assetid: 4F51B0E7-6A63-403C-B488-500CCBCE75DD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
---

# Picking a Photo from the Picture Library

This article walks through the creation of an application that allows the user to pick a photo from the phone's picture library. Because Xamarin.Forms does not include this functionality, it is necessary to use [`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) to access native APIs on each platform.  This article will cover the following steps for using `DependencyService` for this task:

- **[Creating the Interface](#Creating_the_Interface)** &ndash; understand how the interface is created in shared code.
- **[iOS Implementation](#iOS_Implementation)** &ndash; learn how to implement the interface in native code for iOS.
- **[Android Implementation](#Android_Implementation)** &ndash; learn how to implement the interface in native code for Android.
- **[Universal Windows Platform Implementation](#UWP_Implementation)** &ndash; learn how to implement the interface in native code for the Universal Windows Platform (UWP).
- **[Windows Phone Implementation](#Windows_Phone_Implementation)** &ndash; learn how to implement the interface in native code for Windows Phone 8.1.
- **[Implementing in Shared Code](#Implementing_in_Shared_Code)** &ndash; learn how to use `DependencyService` to call into the native implementation from shared code.

<a name="Creating_the_Interface" />

## Creating the Interface

First, create an interface in shared code that expresses the desired functionality. In the case of a photo-picking application, just one method is required. This is defined in the  [`IPicturePicker`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/IPicturePicker.cs) interface in the portable class library of the sample code:

```csharp
namespace DependencyServiceSample
{
    public interface IPicturePicker
    {
        Task<Stream> GetImageStreamAsync();
    }
}
```

The `GetImageStreamAsync` method is defined as asynchronous because the method must return quickly, but it can't return a `Stream` object for the selected photo until the user has browsed the picture library and selected one.

This interface is implemented in all the platforms using platform-specific code.

<a name="iOS_Implementation" />

## iOS Implementation

The iOS implementation of the `IPicturePicker` interface uses the [`UIImagePickerController`](https://developer.xamarin.com/api/type/UIKit.UIImagePickerController/) as described in the [**Choose a Photo from the Gallery**](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/) recipe and [sample code](https://github.com/xamarin/recipes/tree/master/ios/media/video_and_photos/choose_a_photo_from_the_gallery).

The iOS implementation is contained in the [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/iOS/PicturePickerImplementation.cs) class in the iOS project of the sample code. To make this class visible to the `DependencyService` manager, the class must be identified with an [`assembly`] attribute of type `Dependency`, and the class must be public and explicitly implement the `IPicturePicker` interface:

```csharp
[assembly: Dependency (typeof (PicturePickerImplementation))]

namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
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
            viewController.PresentModalViewController(imagePicker, true);

            // Return Task object
            taskCompletionSource = new TaskCompletionSource<Stream>();
            return taskCompletionSource.Task;
        }
        ...
    }
}

```

The `GetImageStreamAsync` method creates a `UIImagePickerController` and initializes it to select images from the photo library. Two event handlers are required: One for when the user selects a photo and the other for when the user cancels the display of the photo library. The `PresentModalViewController` then displays the photo library to the user.

At this point, the `GetImageStreamAsync` method must return a `Task<Stream>` object to the code that's calling it. This task is completed only when the user has finished interacting with the photo library and one of the event handlers is called. For situations like this, the [`TaskCompletionSource`](https://msdn.microsoft.com/library/dd449174(v=vs.110).aspx) class is essential. The class provides a `Task` object of the proper generic type to return from the `GetImageStreamAsync` method, and the class can later be signaled when the task is completed.

The `FinishedPickingMedia` event handler is called when the user has selected a picture. However, the handler provides a `UIImage` object and the `Task` must return a .NET `Stream` object. This is done in two steps: The `UIImage` object is first converted to a JPEG file in memory stored in an `NSData` object, and then the `NSData` object is converted to a .NET `Stream` object. A call to the `SetResult` method of the `TaskComkpletionSource` object completes the task by providing the `Stream` object:

```csharp
namespace DependencyServiceSample.iOS
{
    public class PicturePickerImplementation : IPicturePicker
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
                NSData data = image.AsJPEG(1);
                Stream stream = data.AsStream();

                // Set the Stream as the completion of the Task
                taskCompletionSource.SetResult(stream);
            }
            else
            {
                taskCompletionSource.SetResult(null);
            }
            imagePicker.DismissModalViewController(true);
        }

        void OnImagePickerCancelled(object sender, EventArgs args)
        {
            taskCompletionSource.SetResult(null);
            imagePicker.DismissModalViewController(true);
        }
    }
}

```

An iOS application requires permission from the user to access the phone's photo library. Add the following to the `dict` section of the Info.plist file:

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>Picture Picker uses photo library</string>
```


<a name="Android_Implementation" />

## Android Implementation

The Android implementation uses the technique described in the [**Select an Image**](https://developer.xamarin.com/recipes/android/other_ux/pick_image/) recipe and the [sample code](https://github.com/xamarin/recipes/tree/master/android/other_ux/pick_image). However, the method that is called when the user has selected an image from the picture library is an `OnActivityResult` override in a class that derives from `Activity`. For this reason, the normal [`MainActivity`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/MainActivity.cs) class in the Android project has been supplemented with a field, a property, and an override of the `OnActivityResult` method:

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
{
    ...
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

Like the iOS implementation, the Android implementation uses a `TaskCompletionSource` to signal when the task has been completed. This `TaskCompletionSource` object is defined as a public property in the `MainActivity` class. This allows the property to be referenced in the [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/Droid/PicturePickerImplementation.cs) class in the Android project. This is the class with the `GetImageStreamAsync` method:

```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.Droid
{
    public class PicturePickerImplementation : IPicturePicker
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

This method accesses the `MainActivity` class for several purposes: for the `Instance` property, for the `PickImageId` field, for the `TaskCompletionSource` property, and to call `StartActivityForResult`. This method is defined by the `FormsApplicationActivity` class that is the base class of `MainActivity`.

<a name="UWP_Implementation" />

## UWP Implementation

Unlike the iOS and Android implementations, the implementation of the photo picker for the Universal Windows Platform does not require the `TaskCompletionSource` class. The [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/UWP/PicturePickerImplementation.cs) class uses the [`FileOpenPicker`](/uwp/api/Windows.Storage.Pickers.FileOpenPicker/) class to get access to the photo library. Because the `PickSingleFileAsync` method of `FileOpenPicker` is itself asynchronous, the `GetImageStreamAsync` method can simply use `await` with that method (and other asynchronous methods) and return a `Stream` object:


```csharp
[assembly: Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.UWP
{
    public class PicturePickerImplementation : IPicturePicker
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

<a name="Windows_Phone_Implementation" />

## Windows Phone 8.1 Implementation

The Windows Phone 8.1 implementation is similar to the UWP implementation except for one big difference that has a major impact: The `PickSingleFileAsync` method of `FileOpenPicker` is not available. Instead, the `GetImageStreamAsync` method must call `PickSingleFileAndContinue`. This method returns when the photo library is displayed to the user, but the user's selection of a photo is signaled by a call to the `OnActivated` method of the `App` class.

For this reason, the [`App`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/App.xaml.cs) class created by the Xamarin.Forms project template in the Windows Phone 8.1 project has been supplemented with a property of type `TaskCompletionSource` and an override of the `OnActivated` method:

```csharp
namespace DependencyServiceSample.WinPhone
{
    public sealed partial class App : Application
    {
        ...
        public TaskCompletionSource<Stream> TaskCompletionSource { set; get; }

        protected async override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            IContinuationActivatedEventArgs continuationArgs = args as IContinuationActivatedEventArgs;

            if (continuationArgs != null &&
                continuationArgs.Kind == ActivationKind.PickFileContinuation)
            {
                FileOpenPickerContinuationEventArgs pickerArgs = args as FileOpenPickerContinuationEventArgs;

                if (pickerArgs.Files.Count > 0)
                {
                    // Get the file and a Stream
                    StorageFile storageFile = pickerArgs.Files[0];
                    IRandomAccessStreamWithContentType raStream = await storageFile.OpenReadAsync();
                    Stream stream = raStream.AsStreamForRead();

                    // Set the completion of the Task
                    TaskCompletionSource.SetResult(stream);
                }
                else
                {
                    TaskCompletionSource.SetResult(null);
                }
            }
        }
    }
}
```

The `OnActivated` method might be called for several reasons, including application startup. The code restricts itself to only those calls when the file-open picker has finished, and then obtains a `Stream` object from the `StorageFile`.

The [`PicturePickerImplementation`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/WinPhone/PicturePickerImplementation.cs) class contains the `GetImageStreamAsync` method that creates the `FileOpenPicker` and calls `PickSingleFileAndContainue`:

```csharp
[assembly: Xamarin.Forms.Dependency(typeof(PicturePickerImplementation))]

namespace DependencyServiceSample.WinPhone
{
    public class PicturePickerImplementation : IPicturePicker
    {
        public Task<Stream> GetImageStreamAsync()
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

            // Display the picker for a single file (resumes in OnActivated in App.xaml.cs)
            openPicker.PickSingleFileAndContinue();

            // Create a TaskCompletionSource stored in App.xaml.cs
            TaskCompletionSource<Stream> taskCompletionSource = new TaskCompletionSource<Stream>();
            (Application.Current as App).TaskCompletionSource = taskCompletionSource;

            // Return the Task object
            return taskCompletionSource.Task;
        }
    }
}

```

<a name="Implementing_in_Shared_Code" />

## Implementing in Shared Code

Now that the interface has been implemented for each platform, the application in the common portable class library can take advantage of it.

The [`App`](https://github.com/xamarin/xamarin-forms-samples/blob/master/DependencyService/DependencyServiceSample/DependencyServiceSample/DependencyServiceSample.cs) class creates a `Button` to pick a photo:

```csharp
Button pickPictureButton = new Button
{
    Text = "Pick Photo",
    VerticalOptions = LayoutOptions.CenterAndExpand,
    HorizontalOptions = LayoutOptions.CenterAndExpand
};
stack.Children.Add(pickPictureButton);
```

The `Clicked` handler uses the `DependencyService` class to call `GetImageStreamAsync`. This results in a call in the platform project. If the method returns a `Stream` object, then the handler creates an `Image` element for that picture with a `TabGestureRecognizer`, and replaces the `StackLayout` on the page with that `Image`:

```csharp
pickPictureButton.Clicked += async (sender, e) =>
{
    pickPictureButton.IsEnabled = false;
    Stream stream = await DependencyService.Get<IPicturePicker>().GetImageStreamAsync();

    if (stream != null)
    {
        Image image = new Image
        {
            Source = ImageSource.FromStream(() => stream),
            BackgroundColor = Color.Gray
        };

        TapGestureRecognizer recognizer = new TapGestureRecognizer();
        recognizer.Tapped += (sender2, args) =>
        {
            (MainPage as ContentPage).Content = stack;
            pickPictureButton.IsEnabled = true;
        };
        image.GestureRecognizers.Add(recognizer);

        (MainPage as ContentPage).Content = image;
    }
    else
    {
        pickPictureButton.IsEnabled = true;
    }
};
```

Tapping the `Image` element returns the page to normal.


## Related Links

- [Choose a Photo from the Gallery (iOS)](https://developer.xamarin.com/recipes/ios/media/video_and_photos/choose_a_photo_from_the_gallery/)
- [Select an Image (Android)](https://developer.xamarin.com/recipes/android/other_ux/pick_image/)
- [DependencyService (sample)](https://developer.xamarin.com/samples/xamarin-forms/DependencyService/DependencyServiceSample)
