---
title: "Playing sound with AVAudioPlayer"
description: "This article shows how to use a helper class to control the playback of sound using an AVAudioPlayer."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4A683A94-F75D-4EAF-8497-E9443653250B
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/19/2016
---

# Playing sound with AVAudioPlayer

_This article shows how to use a helper class to control the playback of sound using an AVAudioPlayer._

## About the AVAudioPlayer

The `AVAudioPlayer` class is used to playback audio data from either memory or a file. Apple recommends using this class to play audio in your app unless you are doing network streaming or require low latency audio I/O.

You can use the `AVAudioPlayer` class to do the following:

- Play sounds of any duration with optional looping.
- Play multiple sounds at the same time with optional synchronization.
- Control volume, playback rate and stereo positioning for each sounds playing.
- Support features such as fast forward or rewind.
- Obtain playback level metering data.

`AVAudioPlayer` supports sounds in any audio format provided by iOS, tvOS and macOS such as .aif, .wav or .mp3.

## Playing sounds in macOS

Because macOS supports the same Audio Toolbox classes as iOS, please see our iOS [Playing Sound with AVAudioPlayer](https://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) documentation for the full details of playing audio in a Xamarin.Mac app.



## Related Links

- [AVAudioPlayer Reference](https://developer.apple.com/documentation/avfoundation/avaudioplayer)
