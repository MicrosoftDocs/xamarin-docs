---
title: "Playing Sound in tvOS with AVAudioPlayer in Xamarin"
description: "This article shows how to use a helper class to control the playback of sound using an AVAudioPlayer in a Xamarin.iOS application."
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
---

# Playing Sound in tvOS with AVAudioPlayer in Xamarin

## About the AVAudioPlayer

The `AVAudioPlayer` is used to playback audio data from either memory or a file. Apple recommends using this class to play audio in your app unless you are doing network streaming or require low latency audio I/O.

You can use the `AVAudioPlayer` to do the following:

- Play sounds of any duration with optional looping.
- Play multiple sounds at the same time with optional synchronization.
- Control volume, playback rate and stereo positioning for each sounds playing.
- Support features such as fast forward or rewind.
- Obtain playback level metering data.

`AVAudioPlayer` supports sounds in any audio format provided by iOS, tvOS and OS X such as `.aif`, `.wav` or `.mp3`.

## Playing Sounds in tvOS

Because tvOS supports the same Audio Toolbox classes as iOS, please see our iOS [Playing Sound with AVAudioPlayer](https://github.com/xamarin/recipes/tree/master/Recipes/ios/media/sound/avaudioplayer) documentation for the full details of playing audio in a Xamarin.tvOS app.



## Related Links

- [AVAudioPlayer Reference](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
