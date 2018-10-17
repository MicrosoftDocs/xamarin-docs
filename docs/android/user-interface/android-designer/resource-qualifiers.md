---
title: "Resource Qualifiers and Visualization Options"
description: "This topic explains how to define resources that will be used only when some qualifier values are matched. A simple example is a language-qualified string resource. A string resource can be defined as the default, with other alternative resources defined to be used for additional languages. All resource types can be qualified, including the layout itself."
ms.prod: xamarin
ms.assetid: 2111C18A-3EDA-3787-25E1-3869FF4BE441
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/25/2018
---

# Resource qualifiers and visualization options

_This topic explains how to define resources that will be used only
when some qualifier values are matched. A simple example is a
language-qualified string resource. A string resource can be defined as
the default, with other alternative resources defined to be used for
additional languages. All resource types can be qualified, including
the layout itself._


# [Visual Studio](#tab/windows)

## Resource qualifier options

**Resource qualifier options** can be accessed by clicking the
ellipsis icon to the right of the **Landscape** mode button:

[![Resource Qualifier Options](resource-qualifiers-images/vs/08-resource-qual-opt-sml.png)](resource-qualifiers-images/vs/08-resource-qual-opt.png#lightbox)

This dialog presents pull-down menus for the following resource
qualifiers:

-   **Language** &ndash; Displays available language resources
    and offers an option to add new language/region resources.

-   **UI Mode** &ndash; Lists display modes (such as **Car Dock**
    and **Desk Dock**) as well as layout directions.

Each of these pull-down menus opens new dialog boxes where you
can select and configure resource qualifiers (as explained
next).

### Language

The **Language** pull-down menu lists only those languages that have
resources defined (or **All languages**, which is the default). However,
there is also an **Add language/region...** option that allows you to
add a new language to the list:

[![Add language/region](resource-qualifiers-images/vs/09-add-language-region-sml.png)](resource-qualifiers-images/vs/09-add-language-region.png#lightbox)

When you click **Add language/region...**, the **Select Language**
dialog opens to display drop-down lists of available languages and
regions:

![List of languages](resource-qualifiers-images/vs/10-languages.png "List of languages")

In this example, we have chosen **fr (French)** for the language and
**BE** (Belgium) for the regional dialect of French. Note that the
**Region** field is optional because many languages can be specified
without regard for specific regions. When the **Language** pull-down
menu is opened again, it displays the newly-added language/region
resource:

![Language and Region chosen](resource-qualifiers-images/vs/11-language-region-added.png "Language and Region chosen")

Note that if you add a new language but you do not create new resources
for it, the added language will no longer be shown the next time you
open the project.

### UI Mode

When you click the **UI Mode** pull-down menu, a list of modes is
displayed, such as **Normal**, **Car Dock**, **Desk Dock**,
**Television**, **Appliance**, and **Watch**:


[![UI Mode menu](resource-qualifiers-images/vs/12-ui-mode-sml.png)](resource-qualifiers-images/vs/12-ui-mode.png#lightbox)

Below this list are the night modes **Not Night** and **Night**,
followed by the layout directions **Left to Right** and **Right to
Left** (for information about **Left to Right** and **Right to Left**
options, see
[LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/)).
The last items in the **Resource Qualifier Options** dialog are the
**Round screens** (for use with Android Wear) or **Not Round screens**.
For information about round and non-round screens, see
[Layouts](https://developer.android.com/training/wearables/ui/layouts.html).
For more information about Android UI modes, see
[UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).

## Action Bar settings

The **Action bar settings** icon is available to the left of the
paintbrush (Theme Editor) icon:

![Action Bar settings](resource-qualifiers-images/vs/14-action-bar.png "Action Bar settings")

This icon opens a dialog popover that provides a way to select from one of
three Action Bar modes:

-   **Standard** &ndash; Consists of either a logo or an icon and title text
    with an optional subtitle.

-   **List** &ndash; List navigation mode. Instead of static title text, this
    mode presents a list menu for navigation within the activity (that is,
    it can be presented to the user as a dropdown list).

-   **Tabs** &ndash; Tab navigation mode. Instead of static title text,
    this mode presents a series of tabs for navigation within the
    activity.

## Themes

The **Theme** drop-down menu displays all of the themes defined in the
project. Selecting **More Themes** opens a dialog with a list of all
themes available from the installed Android SDK, as shown below:

[![More Themes list](resource-qualifiers-images/vs/15-theme-menu-sml.png "More Themes list")](resource-qualifiers-images/vs/15-theme-menu.png#lightbox)

When a theme is selected, the Design Surface is updated to show
the effect of the new theme. Note that this change is made permanent only
if the **OK** button is clicked in the **Theme** dialog. Once a theme
has been selected, it will be included in the **Theme** drop-down menu
as shown below:

![Light theme is now available](resource-qualifiers-images/vs/16-light-theme.png "Light theme is now available")

## Android version

The Android **Version** selector sets the Android version that is used
to render the layout in the Designer. The selector displays all
versions that are compatible with the target framework version of the
project:

![List of Android versions](resource-qualifiers-images/vs/17-android-version.png "List of Android versions")

The target framework version can be set in the project's settings under
**Properties > Application > Compile using Android version**. For more
information about target framework version, see
[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md).

The set of widgets available in the toolbox is determined by the target
framework version of the project. This is also true for the properties
available in the **Properties Window**. The available list of widgets 
is *not* determined by the value selected in the **Version** selector of
the toolbar. For example, if you set the target version of the project
to Android 4.4, you can still select Android 6.0 in the toolbar version
selector to see what the project looks like in Android 6.0, but you
won't be able to add widgets that are specific to Android 6.0 &ndash;
you will still be limited to the widgets that are available in Android
4.4.

For more information about resource types, see
[Android Resources](~/android/app-fundamentals/resources-in-android/index.md).



# [Visual Studio for Mac](#tab/macos)

## Resource qualifier options

**Resource qualifier options** can be accessed by clicking the
ellipsis icon to the right of the **Landscape** mode button:

[![Resource qualifier options](resource-qualifiers-images/xs/08-resource-qual-opt-m75-sml.png)](resource-qualifiers-images/xs/08-resource-qual-opt-m75.png#lightbox)

This dialog presents pull-down menus for the following resource
qualifiers:

-   **Language** &ndash; Displays available language resources
    and offers an option to add new language/region resources.

-   **UI Mode** &ndash; Lists display modes (such as **Car Dock**
    and **Desk Dock**) as well as layout directions.

Each of these pull-down menus opens new dialog boxes where you
can select and configure resource qualifiers (as explained
next).

### Language

The **Language** pull-down menu lists only those languages that have
resources defined (or **All languages**, which is the default). However,
there is also an **Add language/region...** option that allows you to
add a new language to the list:

[![Add language/region](resource-qualifiers-images/xs/09-add-language-region-m75-sml.png)](resource-qualifiers-images/xs/09-add-language-region-m75.png#lightbox)

When you click **Add language/region...**, the **Select Language**
dialog opens to display drop-down lists of available languages and
regions:

[![List of languages](resource-qualifiers-images/xs/10-languages-m75-sml.png)](resource-qualifiers-images/xs/10-languages-m75.png#lightbox)

In this example, we have chosen **fr (French)** for the language and
**BE** (Belgium) for the regional dialect of French. Note that the
**Region** field is optional because many languages can be specified
without regard for specific regions. When the **Language** pull-down
menu is opened again, it displays the newly-added language/region
resource:

[![Language and Region Chosen](resource-qualifiers-images/xs/11-language-region-added-m75-sml.png)](resource-qualifiers-images/xs/11-language-region-added-m75.png#lightbox)

Note that if you add a new language but you do not create new resources
for it, the added language will no longer be shown the next time you
open the project.

### UI Mode

When you click the **UI Mode** pull-down menu, a list of modes is
displayed, such as **Normal**, **Car Dock**, **Desk Dock**,
**Television**, **Appliance**, and **Watch**:

[![UI Mode menu](resource-qualifiers-images/xs/12-ui-mode-m75-sml.png)](resource-qualifiers-images/xs/12-ui-mode-m75.png#lightbox)

Below this list are the night modes **Not Night** and **Night**,
followed by the layout directions **Left to Right** and **Right to
Left**. The last pair of options allows you to select either **Round
screens** or **Rectangular screens** (useful for Android Wear devices).

For more information about Android UI modes, see
[UiModeManager](https://developer.xamarin.com/api/type/Android.App.UiModeManager/).
For information about **Left to Right** and **Right to Left** options,
see [LayoutDirection](https://developer.xamarin.com/api/type/Android.Util.LayoutDirection/).


## Action Bar settings

The **Action bar settings** icon is available to the left of the
paintbrush (Theme Editor) icon:

[![Action Bar settings](resource-qualifiers-images/xs/13-action-bar-m75-sml.png)](resource-qualifiers-images/xs/13-action-bar-m75.png#lightbox)

This icon opens a dialog popover that provides a way to select from one of
three Action Bar modes:

-   **Standard** &ndash; Consists of either a logo or icon and title text
    with an optional subtitle.

-   **List** &ndash; List navigation mode. Instead of static title text, this
    mode presents a list menu for navigation within the activity (that is,
    it can be presented to the user as a dropdown list).

-   **Tabs** &ndash; Tab navigation mode. Instead of static title text,
    this mode presents a series of tabs for navigation within the
    activity.

## Themes

The **Theme** drop-down menu displays all of the themes defined in the
project. Selecting **More Themes** opens a dialog with a list of all
themes available from the installed Android SDK, as shown below:

[![More Themes list](resource-qualifiers-images/xs/14-theme-menu-m75-sml.png)](resource-qualifiers-images/xs/14-theme-menu-m75.png#lightbox)

When a theme is selected, the Design Surface is updated to show
the effect of the new theme. Note that this change is made permanent only
if the **OK** button is clicked in the **Theme** dialog. Once a theme
has been selected, it will be included in the **Theme** drop-down menu
as shown below:

[![Light theme is now available](resource-qualifiers-images/xs/15-light-theme-m75-sml.png)](resource-qualifiers-images/xs/15-light-theme-m75.png#lightbox)

## Android version

The Android **Version** selector sets the Android version that is used
to render the layout in the Designer. The selector displays all
versions that are compatible with the target framework version of the
project:

[![List of Android versions](resource-qualifiers-images/xs/16-android-version-m75-sml.png)](resource-qualifiers-images/xs/16-android-version-m75.png#lightbox)

The target framework version can be set in the project's settings under
the **Project Options > Build > General** section. For more information
about target framework version, see
[Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md).

The set of widgets available in the toolbox is determined by the target
framework version of the project. This is also true for the properties
available in the **Property Pad**. The available list of widgets 
is *not* determined by the value selected in the **Version** selector of
the toolbar. For example, if you set the target version of the project
to Android 4.4, you can still select Android 6.0 in the toolbar version
selector to see what the project looks like in Android 6.0, but you
won't be able to add widgets that are specific to Android 6.0 &ndash;
you will still be limited to the widgets that are available in Android
4.4.

For more information about resource types, see
[Android Resources](~/android/app-fundamentals/resources-in-android/index.md).

-----
