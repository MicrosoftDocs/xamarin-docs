---
title: "Editing Android Virtual Device Properties"
description: "This article explains how to use the Android Device Manager to edit the profile properties of an Android virtual device."
zone_pivot_groups: platform
ms.prod: xamarin
ms.assetid: 3E33C136-8042-4184-A40C-3200D8CD99CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2018
---

# Editing Android Virtual Device Properties

_This article explains how to use the Android Device Manager to edit the
profile properties of an Android virtual device._

::: zone pivot="windows"

## Android Device Manager on Windows

The **Android Device Manager** supports the editing of individual
Android virtual device profile properties. The **New Device** and
**Device Edit** screens list the properties of the virtual device in
the first column, with the corresponding values of each property in the
second column (as seen in this example): 

[![Example New Device screen](device-properties-images/win/01-new-device-editor-sml.png)](device-properties-images/win/01-new-device-editor.png#lightbox)

When you select a property, a detailed description of that property is
displayed on the right. You can modify *hardware profile properties*
and *AVD properties*. Hardware profile properties (such as `hw.ramSize`
and `hw.accelerometer`) describe the physical characteristics of the
emulated device. These characteristics include screen size, the amount
of available RAM, whether or not an accelerometer is present. AVD
properties specify the operation of the AVD when it runs. For example,
AVD properties can be configured to specify how the AVD uses your
development computer's graphics card for rendering.

You can change properties by using the following guidelines:

-   To change a boolean property, click the check mark to the right of
    the boolean property:

    ![Changing a boolean property](device-properties-images/win/02-boolean-value.png)

-   To change an *enum* (enumerated) property, click the down-arrow to
    the right of the property and choose a new value.

    ![Changing an enum property](device-properties-images/win/04-enum-value.png)

-   To change a string or integer property, double-click the current
    string or integer setting in the value column and enter a new value.

    ![Changing an integer property](device-properties-images/win/03-integer-value.png)

::: zone-end
::: zone pivot="macos"

## Android Device Manager on macOS

The **Android Device Manager** supports the editing of individual
Android virtual device profile properties. The **New Device** and
**Device Edit** screens list the properties of the virtual device in
the first column, with the corresponding values of each property in the
second column (as seen in this example): 

[![Example New Device screen](device-properties-images/mac/01-new-device-editor-sml.png)](device-properties-images/mac/01-new-device-editor.png#lightbox)

When you select a property, a detailed description of that property is
displayed on the right. You can modify *hardware profile properties*
and *AVD properties*. Hardware profile properties (such as `hw.ramSize`
and `hw.accelerometer`) describe the physical characteristics of the
emulated device. These characteristics include screen size, the amount
of available RAM, whether or not an accelerometer is present. AVD
properties specify the operation of the AVD when it runs. For example,
AVD properties can be configured to specify how the AVD uses your
development computer's graphics card for rendering.

You can change properties by using the following guidelines:

-   To change a boolean property, click the check mark to the right of
    the boolean property:

    ![Changing a boolean property](device-properties-images/mac/02-boolean-value.png)

-   To change an *enum* (enumerated) property, click the pull-down menu
    to the right of the property and choose a new value.

    ![Changing an enum property](device-properties-images/mac/04-enum-value.png)

-   To change a string or integer property, double-click the current
    string or integer setting in the value column and enter a new value.

    ![Changing an integer property](device-properties-images/mac/03-integer-value.png)

::: zone-end

The following table provides a detailed explanation of the properties
listed in the **New Device** and **Device Editor** screens:

[!include[](~/android/includes/emulator-properties.md)]

For more information about these properties, see
[Hardware Profile Properties](https://developer.android.com/studio/run/managing-avds.html#hpproperties).

