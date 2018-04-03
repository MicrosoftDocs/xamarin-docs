---
title: "Emotion Recognition Using the Face API"
description: "The Face API takes a facial expression in an image as an input, and returns data that includes confidence levels across a set of emotions for each face in the image. This article explains how to use the Face API to recognize emotion, to rate a Xamarin.Forms application."
ms.prod: xamarin
ms.assetid: 19D36A7C-E8D8-43D1-BE80-48DE6C02879A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
---

# Emotion Recognition Using the Face API

_The Face API takes a facial expression in an image as an input, and returns data that includes confidence levels across a set of emotions for each face in the image. This article explains how to use the Face API to recognize emotion, to rate a Xamarin.Forms application._

## Overview

The Face API can perform emotion detection to detect anger, contempt, disgust, fear, happiness, neutral, sadness, and surprise, in a facial expression. These emotions are universally and cross-culturally communicated via the same basic facial expressions. As well as returning an emotion result for a facial expression, the Face API can also returns a bounding box for detected faces. Note that an API key must be obtained to use the Face API. This can be obtained at [Try Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

Emotion recognition can be performed via a client library, and via a REST API. This article focuses on performing emotion recognition via the [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/) client library, which can be downloaded from NuGet.

The Face API can also be used to recognize the facial expressions of people in video, and can return a summary of their emotions. For more information, see [How to Analyze Videos in Real-time](/azure/cognitive-services/face/face-api-how-to-topics/howtoanalyzevideo_face/).

For more information about the Face API, see [Face API](/azure/cognitive-services/face/overview/).

## Performing Emotion Recognition

Emotion recognition is achieved by uploading an image stream to the Face API. The image file size shouldn't be larger than 4MB, and supported file formats are JPEG, PNG, GIF, and BMP.

The following code example shows the emotion recognition process:

```csharp
using Microsoft.ProjectOxford.Face;
using Microsoft.ProjectOxford.Face.Contract;

var	faceServiceClient = new FaceServiceClient(Constants.FaceApiKey, Constants.FaceEndpoint);
// e.g. var faceServiceClient = new FaceServiceClient("a3dbe2ed6a5a9231bb66f9a964d64a12", "https://westus.api.cognitive.microsoft.com/face/v1.0/detect");

var faceAttributes = new FaceAttributeType[] { FaceAttributeType.Emotion };
using (var photoStream = photo.GetStream())
{
    Face[] faces = await faceServiceClient.DetectAsync(photoStream, true, false, faceAttributes);
    if (faces.Any())
    {
        // Emotions detected are happiness, sadness, surprise, anger, fear, contempt, disgust, or neutral.
        emotionResultLabel.Text = faces.FirstOrDefault().FaceAttributes.Emotion.ToRankedList().FirstOrDefault().Key;
    }
    // Store emotion as app rating
    ...
}
```

An `FaceServiceClient` instance must be created to perform emotion recognition, with the Face API key and endpoint being passed as arguments to the `FaceServiceClient` constructor.

> [!NOTE]
> You must use the same region in your Face API calls as you used to obtain your subscription keys. For example, if you obtained your subscription keys from the `westus` region, the face detection endpoint will be `https://westus.api.cognitive.microsoft.com/face/v1.0/detect`.

The `DetectAsync` method, which is invoked on the `FaceServiceClient` instance, uploads an image to the Face API, as a `Stream`. The API key will be submitted to the Face API when this operation is invoked. Failure to submit a valid API key will result in a `Microsoft.ProjectOxford.Face.FaceAPIException` being thrown, with the exception message indicating that an invalid API key was submitted.

The `DetectAsync` method will return an `Face` array, provided that a face has been recognized. Each returned face contains a rectangle to indicate its location, combined with a series of optional face attributes, which are specified by the `faceAttributes` argument to the `DetectAsync` method. If no face is detected, an empty `Face` array will be returned.

When interpreting results from the Face API, the detected emotion should be interpreted as the emotion with the highest score, as scores are normalized to sum to one. Therefore, the sample application displays the recognized emotion with the highest score for the largest detected face in the image, as shown in the following screenshots:

![](emotion-recognition-images/emotion-recognition.png "Emotion Recognition")

## Summary

This article explained how to use the Face API to recognize emotion, to rate a Xamarin.Forms application. The Face API takes a facial expression in an image as an input, and returns data that includes the confidence across a set of emotions for each face in the image.

## Related Links

- [Face API](/azure/cognitive-services/face/overview/).
- [Todo Cognitive Services (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Microsoft.ProjectOxford.Face](https://www.nuget.org/packages/Microsoft.ProjectOxford.Face/)
