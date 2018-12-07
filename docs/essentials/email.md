---
title: "Xamarin.Essentials: Email"
description: "The Email class in Xamarin.Essentials enables an application to open the default email application with a specified information including subject, body, and recipients (TO, CC, BCC)."
ms.assetid: 5FBB6FF0-0E7B-4C29-8F06-91642AF12629
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
---

# Xamarin.Essentials: Email

The **Email** class enables an application to open the default email application with a specified information including subject, body, and recipients (TO, CC, BCC).

## Get started

[!include[](~/essentials/includes/get-started.md)]

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


## Platform Differences

# [Android](#tab/android)

Not all email clients for Android support `Html`, since there is no way to detect this we recommend using `PlainText` when sending emails.

# [iOS](#tab/ios)

No platform differences.

# [UWP](#tab/uwp)

Only supports `PlainText` as the `BodyFormat` attempting to send `Html` will throw a `FeatureNotSupportedException`.

-----

## API

- [Email source code](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Email)
- [Email API documentation](xref:Xamarin.Essentials.Email)
