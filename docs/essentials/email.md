---
title: "Xamarin.Essentials: Email"
description: "The Email class in Xamarin.Essentials enables an application to open the default email application with a specified information including subject, body, and recipients (TO, CC, BCC)."
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
---

# Xamarin.Essentials: Email

The **Email** class enables an application to open the default email application with a specified information including subject, body, and recipients (TO, CC, BCC).

## Get started

[!include[](~/essentials/includes/get-started.md)]

> [!TIP]
> To use the Email API on iOS you must run it on a physical device, else an exception will be thrown.

## Using Email

Add a reference to Xamarin.Essentials in your class:

```csharp
using Xamarin.Essentials;
```

The Email functionality works by calling the `ComposeAsync` method an `EmailMessage` that contains information about the email:

```csharp
public class EmailTest
{
    public async Task SendEmail(string subject, string body, List<string> recipients)
    {
        try
        {
            var message = new EmailMessage
            {
                Subject = subject,
                Body = body,
                To = recipients,
                //Cc = ccRecipients,
                //Bcc = bccRecipients
            };
            await Email.ComposeAsync(message);
        }
        catch (FeatureNotSupportedException fbsEx)
        {
            // Email is not supported on this device
        }
        catch (Exception ex)
        {
            // Some other exception occurred
        }
    }
}
```


## File Attachments

![Preview feature](~/media/shared/preview.png)

Emailing files is available as an experimental preview in Xamarin.Essentials version 1.1.0. This features enables an app to emails files in email clients on the device. To enable this feature set the following property in your app's startup code:

```csharp
ExperimentalFeatures.Enable(ExperimentalFeatures.EmailAttachments);
```

After the feature enabled any file can be emailed. Xamarin.Essentials will automatically detect the file type (MIME) and request the file to be added as an attachment. Every email client is different a may only support specific file extensions or none at all.

Here is a sample of writing text to disk and adding it as an email attachment:

```csharp
var message = new EmailMessage
{
    Subject = "Hello",
    Body = "World",
};

var fn = "Attachment.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

message.Attachments.Add(new EmailAttachment(file));

await Email.ComposeAsync(message);
```

## Platform Differences

# [Android](#tab/android)

Not all email clients for Android support `Html`, since there is no way to detect this we recommend using `PlainText` when sending emails.

# [iOS](#tab/ios)

No platform differences.

# [UWP](#tab/uwp)

Only supports `PlainText` as the `BodyFormat` attempting to send `Html` will throw a `FeatureNotSupportedException`.

Not all email clients support sending attachments. See [documentation](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email) for more inforamtion.

-----

## API

- [Email source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Email API documentation](xref:Xamarin.Essentials.Email)
