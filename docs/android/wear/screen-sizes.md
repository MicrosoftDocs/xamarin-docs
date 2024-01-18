---
title: "Working with Screen Sizes in Xamarin.Android and Wear OS"
description: Learn how to work with various screen sizes in Xamarin.Android and Wear OS.
ms.service: xamarin
ms.assetid: 77831169-C663-4D42-B742-B8B556B1DA4B
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 04/25/2018
---

# Working with Screen Sizes

Android Wear devices can have either a rectangular or a round display,
which can also be different sizes.

![Screenshots of rectangular and round Wear displays](screen-sizes-images/moyeu-wear.png)

## Identifying Screen Type

The Wear support library provides some controls that help you detect
and adapt to different screen shapes, such as `WatchViewStub` and
`BoxInsetLayout`.

Be aware that some of the other support library controls (such as
`GridViewPager`) *automatically* detect screen shape themselves and
shouldn't be added as children of the controls described below.

### WatchViewStub

See the
[WatchViewStub](/samples/xamarin/monodroid-samples/wear-watchviewstub) sample to see how to detect
screen type and display a different layout for each type.

The main layout file contains a
`android.support.wearable.view.WatchViewStub` which references
different layouts for rectangular and round screens using the
`app:rectLayout` and `app:roundLayout` attributes:

```xml
<android.support.wearable.view.WatchViewStub
    xmlns:app="http://schemas.android.com/apk/res-auto"
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:id="@+id/stub"
  app:rectLayout="@layout/rect_layout"
  app:roundLayout="@layout/round_layout" />
```

The solution contains different layouts for each style which will be
selected at run-time:

![Files shown under Resources/layout](screen-sizes-images/solution.png)

### BoxInsetLayout

Rather than build different layouts for each screen type, you can also
create a single view that adapts to rectangular or round screens.

This
[Google example](https://developer.android.com/training/wearables/ui/layouts.html#same-layout)
shows how to use the `BoxInsetLayout` to use the same layout on both
rectangular and round screens.

## Wear UI Designer

The Xamarin Android Designer supports both rectangular and round
screens:

![Selecting the Android Wear Square screen in the Xamarin Android Designer](screen-sizes-images/design-screen-type.png)

The design surface in rectangular style is shown here:

![Design surface in rectangular style](screen-sizes-images/design-rect.png) 

The design surface in round style is shown here:

![Design surface in round style](screen-sizes-images/design-round.png)

## Wear Simulator

The **Google Emulator Manager** contains device definitions for both
screen types. You can create rectangular and round emulators to test
your app.

![Wear device definitions shown in the Google Emulator Manager](screen-sizes-images/emulator-devices.png)

The emulator will render like this for a rectangular screen:

![Emulator rendering of a rectangular screen](screen-sizes-images/recipe-2.png) 

It will render like this for a round screen:

![Emulator rendering of a round screen](screen-sizes-images/recipe-2-round.png)

## Video

[Fullscreen apps for Android Wear](https://www.youtube.com/watch?v=naf_WbtFAlY) from
[developers.google.com](https://www.youtube.com/channel/UC_x5XG1OV2P6uZZ5FSM9Ttw).