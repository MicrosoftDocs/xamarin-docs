---
title: "Speech Recognition Using the Bing Speech API"
description: "The Bing Speech API is a cloud-based API that provides algorithms to process spoken language. This article explains how to use the Bing Speech Recognition REST API to convert audio to text in a Xamarin.Forms application."
ms.topic: article
ms.prod: xamarin
ms.assetid: B435FF6B-8785-48D9-B2D9-1893F5A87EA1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
---

# Speech Recognition Using the Bing Speech API

_The Bing Speech API is a cloud-based API that provides algorithms to process spoken language. This article explains how to use the Bing Speech Recognition REST API to convert audio to text in a Xamarin.Forms application._

![](~/media/shared/preview.png "This API is currently pre-release")

> [!NOTE]
> The Bing Speech API is still in preview. There may be breaking changes to the API before the final release.

## Overview

The Bing Speech API has two components:

- A speech recognition API for converting spoken words to text. Speech recognition can be performed via a REST API, client library, or service library.
- A text to speech API for converting text into spoken words. Text to speech conversion is performed via a REST API.

This article focuses on performing speech recognition via the REST API. While the client and service libraries support returning partial results, the REST API can only return a single recognition result, without any partial results.

An API key must be obtained to use the Bing Speech Recognition API. This can be obtained at [Getting started for free](https://www.microsoft.com/cognitive-services/en-US/sign-up?ReturnUrl=/cognitive-services/en-us/subscriptions?productId=%2fproducts%2fBing.Speech.Preview) on microsoft.com.

For more information about the Bing Speech API, see [Bing Speech Documentation](https://www.microsoft.com/cognitive-services/en-us/Speech-api/documentation/overview) on microsoft.com.

## Authentication

Every request made to the Bing Speech Recognition REST API requires a JSON Web Token (JWT) access token, which can be obtained from the cognitive services token service at `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. A token can be obtained by making a POST request to the token service, specifying an `Ocp-Apim-Subscription-Key` header that contains the API key as its value.

The following code example shows how to request an access token from the token service:

```csharp
async Task<string> FetchTokenAsync(string fetchUri, string apiKey)
{
  using (var client = new HttpClient())
  {
    client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";

    var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
  }
}
```

The returned access token, which is Base64 text, has an expiry time of 10 minutes. Therefore, the sample application renews the access token every 9 minutes.

The access token must be specified in each Bing Speech Recognition REST API call as an `Authorization` header prefixed with the string `Bearer`, as shown in the following code example:

```csharp
using (var httpClient = new HttpClient())
{
  httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
  ...
}  
```

Failure to pass a valid access token to the Bing Speech Recognition REST API will result in a 403 response error.

## Performing Speech Recognition

Speech recognition is achieved by making a POST request to `recognize` API at `https://speech.platform.bing.com/recognize`. A single request can't contain more than 10 seconds of audio, and the total request duration can't exceed 14 seconds.

Audio content must be placed in the POST body of the request in wav format. For information about supported codecs, see [Supported Codecs](https://www.microsoft.com/cognitive-services/en-us/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-codecs) on microsoft.com.

In the sample application, the `RecognizeSpeechAsync` method invokes the speech recognition process:

```csharp
public async Task<SpeechResult> RecognizeSpeechAsync(string filename)
{
    ...

	// Read audio file to a stream
	var file = await PCLStorage.FileSystem.Current.LocalStorage.GetFileAsync(filename);
	var fileStream = await file.OpenAsync(PCLStorage.FileAccess.Read);

	// Send audio stream to Bing and deserialize the response
	string requestUri = GenerateRequestUri(Constants.SpeechRecognitionEndpoint);
	string accessToken = authenticationService.GetAccessToken();
	var response = await SendRequestAsync(fileStream, requestUri, accessToken, Constants.AudioContentType);
	var speechResults = JsonConvert.DeserializeObject<SpeechResults>(response);

	fileStream.Dispose();
	return speechResults.results.FirstOrDefault();
}
```

Audio is recorded in each platform-specific project as PCM wav data, and the `RecognizeSpeechAsync` method uses the `PCLStorage` NuGet package to open the audio file as a stream. The speech recognition request URI is generated and an access token is retrieved from the token service. The speech recognition request is posted to the `recognize` API, which returns a JSON response containing the result. The JSON response is deserialized, with the result being returned to the calling method for display.

### Configuring Speech Recognition

The speech recognition process can be configured by specifying HTTP query parameters. There are compulsory and optional parameters, with the following method showing the compulsory parameters that must be set:

```csharp
string GenerateRequestUri(string speechEndpoint)
{
	string requestUri = speechEndpoint;
	requestUri += @"?scenarios=ulm";                                    // websearch is the other option
	requestUri += @"&appid=D4D52672-91D7-4C74-8AD8-42B1D98141A5";       // You must use this ID
	requestUri += @"&locale=en-US";                                     // Other languages supported
	requestUri += string.Format("&device.os={0}", operatingSystem);     // Open field
	requestUri += @"&version=3.0";                                      // Required value
	requestUri += @"&format=json";                                      // Required value
	requestUri += @"&instanceid=fe34a4de-7927-4e24-be60-f0629ce1d808";  // GUID for device making the request
	requestUri += @"&requestid=" + Guid.NewGuid().ToString();           // GUID for the request
	return requestUri;
}
```

The main configuration performed by the `GenerateRequestUri` method is to set the locale of the audio content. For a list of the supported locales, see [Supported Locales ](https://www.microsoft.com/cognitive-services/en-us/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#supported-locales) on microsoft.com.

For more information about the possible values for the compulsory parameters, see [Required Parameters](https://www.microsoft.com/cognitive-services/en-us/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#required-parameters) on microsoft.com. For information about the optional parameters, see [Optional Parameters](https://www.microsoft.com/cognitive-services/en-us/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition) on microsoft.com.

### Sending the Request

The `SendRequestAsync` method makes the POST request to the Bing Speech Recognition REST API and returns the response:

```csharp
async Task<string> SendRequestAsync(Stream fileStream, string url, string bearerToken, string contentType)
{
	var content = new StreamContent(fileStream);
	content.Headers.TryAddWithoutValidation("Content-Type", contentType);

	using (var httpClient = new HttpClient())
	{
		httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
		var response = await httpClient.PostAsync(url, content);

		return await response.Content.ReadAsStringAsync();
	}
}
```

This method builds the POST request by:

- Wrapping the audio stream in a `StreamContent` instance, which provides HTTP content based on a stream.
- Setting the `Content-Type` header of the request to `audio/wav; codec="audio/pcm"; samplerate=16000`.
- Adding the access token to the `Authorization` header, prefixed with the string `Bearer`.

The POST request is then sent to `recognize` API. The response is then read and returned to the calling method.

The `recognize` API will send HTTP status code 200 (OK) in the response, provided that the request is valid, which indicates that the request succeeded and that the requested information is in the response. For a list of possible error responses, see [Error Responses](https://www.microsoft.com/cognitive-services/en-us/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#error-responses) on microsoft.com.

### Processing the Response

The API response is returned in JSON format, with the recognized text being contained in the `name` tag. The following JSON data shows a typical successful response message:

```csharp
{
  "version": "3.0",
  "header": {
    "status": "success",
    "scenario": "ulm",
    "name": "go shopping tomorrow",
    "lexical": "go shopping tomorrow",
    "properties": {
      "requestid": "e06c059d-fa37-4bb1-843f-4914350279a8",
      "HIGHCONF": "1"
    }
  },
  "results": [
    {
      "scenario": "ulm",
      "name": "go shopping tomorrow",
      "lexical": "go shopping tomorrow",
      "confidence": "0.9493451",
      "properties": {
        "HIGHCONF": "1"
      }
    }
  ]
}
```

In the sample application, the JSON response is deserialized into a `SpeechResult` instance, with the result being returned to the calling method for display, as shown in the following screenshots:

![](speech-recognition-images/speech-recognition.png "Speech Recognition")

For information about the values of each JSON tag, see [Speech Recognition Responses](https://www.microsoft.com/cognitive-services/en-us/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition#speech-recognition-responses) on microsoft.com.

## Summary

This article explained how to use the Bing Speech Recognition REST API to convert audio to text in a Xamarin.Forms application. In addition to performing speech recognition, the Bing Speech API can also convert text into spoken words.



## Related Links

- [Bing Speech Documentation](https://www.microsoft.com/cognitive-services/en-us/Speech-api/documentation/overview)
- [Consuming a RESTful Web Service](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Todo Cognitive Services (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing Speech Recognition REST API](https://www.microsoft.com/cognitive-services/en-us/Speech-api/documentation/API-Reference-REST/BingVoiceRecognition)
