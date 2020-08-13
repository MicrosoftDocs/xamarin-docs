---
title: "Accessibility in Xamarin Apps"
description: "This document provides various tips for the creation of accessible apps. For example, it includes recommendations about large fonts, high contrast, self-describing interfaces, and more."
ms.prod: xamarin
ms.assetid: E587F0CF-7C1D-41F8-B5A8-DA3E738EDA81
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
---

# Accessibility in Xamarin Apps

_Ensure that your apps are useable by the widest possible audience_

Accessibility refers to the concept of designing app user interfaces
that work well operating system display- and input-assistance features
such as large type, high contrast, zoom in, screen reading
(text-to-speech), visual or haptic feedback cues, and alternative input methods.

Desktop and mobile platforms like iOS, Android, and Windows provide
built in APIs that help developers build accessible apps, such as
[Google TalkBack](https://play.google.com/store/apps/details?id=com.google.android.marvin.talkback) and
[Apple's VoiceOver](https://www.apple.com/accessibility/ios/voiceover/).

## Platform-Specific APIs

To implement the guidelines in this document, use the APIs provided
by each platform:

- [**Android Accessibility**](~/android/app-fundamentals/accessibility.md)
- [**iOS Accessibility**](~/ios/app-fundamentals/accessibility.md)
- [**OS X Accessibility**](~/mac/app-fundamentals/accessibility.md)
- [**Xamarin.Forms**](~/xamarin-forms/app-fundamentals/accessibility/index.md)

<a name="checklist"></a>

## Accessibility Checklist

Follow these tips to ensure that your apps are accessible to
the widest audience possible. Check out the [Android Accessibility Testing Checklist](https://developer.android.com/training/accessibility/testing.html)
and [Apple's Accessibility page](https://www.apple.com/accessibility/)
for additional information.

### Support large fonts and high contrast

Avoid hardcoding control dimensions and, instead, prefer
layouts that can resize to accommodate larger font sizes.
Test color schemes in high-contrast mode
to ensure that they are readable.

### Make the user interface self-describing

Tag all the elements of your user-interface with
descriptive text and hints that are compatible
with the screen reading APIs on each platform.

### Ensure that images and icons have an alternate text description

Images and icons that are part of the application
user interface (such as buttons or indicators of status, for example)
should be tagged with an accessible description.

### Design the visual tree with accessible navigation in mind

Use appropriate layout controls or APIs so that
navigating between controls using alternate input
methods follows the same logical flow as using
the touch screen.

Exclude unnecessary elements from screen readers
(decorative images or labels for fields that are
already accessible, for example).

### Don't rely on audio or color cues alone

Avoid situations where the sole indication of
progress, completion, or some other state is a sound or
color-change. Either design the user interface to
include clear visual cues (with sound and color for
reinforcement only), or add specific accessibility indicators.

When choosing colors, try to avoid a palette that
is hard to distinguish for users with color blindness.

### Captioning for video, text for audio

Provide captions for video content, and a readable
script for audio content. It's also helpful
to provide controls that adjust the speed of audio or video
content, and ensure that volume and play/pause buttons
are easy to find and use.

### Localize

Accessibility descriptions can (and should) be localized
where the application supports multiple languages.

## Related Links

- [Android Accessibility](~/android/app-fundamentals/accessibility.md)
- [iOS Accessibility](~/ios/app-fundamentals/accessibility.md)
- [OS X Accessibility](~/mac/app-fundamentals/accessibility.md)
- [Xamarin.Forms Accessibility](~/xamarin-forms/app-fundamentals/accessibility/index.md)
