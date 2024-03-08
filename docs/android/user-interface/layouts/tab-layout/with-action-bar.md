---
title: "Tabbed Layouts with the ActionBar"
description: "This guide introduces and explains how to use the ActionBar APIs to create a tabbed user interface in a Xamarin.Android application."
ms.service: xamarin
ms.assetid: B7E60AAF-BDA5-4305-9000-675F0438734D
ms.subservice: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
---

# Tabbed Layouts with the ActionBar

_This guide introduces and explains how to use the ActionBar APIs to create a tabbed user interface in a Xamarin.Android application._

## Overview

The action bar is an Android UI pattern that is used to provide a 
consistent user interface for key features such as tabs, application 
identity, menus, and search. In Android 3.0 (API level 11), Google 
introduced the ActionBar APIs to the Android platform. The ActionBar 
APIs introduce UI themes to provide a consistent look and feel and 
classes that allow for tabbed user interfaces. This guide discusses how 
to add Action Bar tabs to a Xamarin.Android application. It also 
discusses how to use the Android Support Library v7 to backport 
ActionBar tabs to Xamarin.Android applications targeting Android 2.1 to 
Android 2.3. 

Note that `Toolbar` is a newer and more generalized action bar component
that you should use instead of `ActionBar` (`Toolbar` was designed to
replace `ActionBar`). For more information, see
[Toolbar](~/android/user-interface/controls/tool-bar/index.md). 

## Requirements

Any Xamarin.Android application that targets API level 11 (Android 3.0) 
or higher has access to the ActionBar APIs as a part of the native 
Android APIs. 

Some of the ActionBar APIs have been back ported to API level 7
(Android 2.1) and are available via the
[V7 AppCompat Library](https://developer.android.com/tools/support-library/features.html#v7-appcompat),
which is made available to Xamarin.Android apps via the
[Xamarin Android Support Library - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
package.

## Introducing Tabs in the ActionBar

The action bar tries to display all of its tabs concurrently and make 
all the tabs equal in size based on the width of the widest tab label. 
This is illustrated by the following screenshot: 

![Example screenshot of ActionBar with all equal-sized tabs shown](with-action-bar-images/image1.png)

When the ActionBar can't display all of the tabs, it will set up the 
tabs in a horizontally scrollable view. The user may swipe left or 
right to see the remaining tabs. This screenshot from Google Play shows 
an example of this: 

![Example screenshot of tabs in a horizontally scrollable view](with-action-bar-images/image2.png)

Each tab in the action bar should be associated with a 
[*fragment*](~/android/platform/fragments/index.md). When the 
user selects a tab, the application will display the fragment that is 
associated with the tab. The ActionBar is not responsible for 
displaying the appropriate fragment to the user. Instead, the ActionBar 
will notify an application about state changes in a tab through a class 
that implements the ActionBar.ITabListener interface. This interface 
provides three callback methods that Android will invoke when the state 
of the tab changes: 

- **OnTabSelected** - This method is called when the user selects the
   tab. It should display the fragment.

- **OnTabReselected** - This method is called when the tab is already
   selected but is selected again by the user. This callback is
   typically used to refresh/update the displayed fragment.

- **OnTabUnselected** - This method is called when the user selects
   another tab. This callback is used to save the state in the
   displayed fragment before it disappears.

Xamarin.Android wraps the `ActionBar.ITabListener` with events on the 
`ActionBar.Tab` class. Applications may assign event handlers to one or 
more of these events. There are three events (one for each method in 
`ActionBar.ITabListener`) that an action bar tab will raise: 

- TabSelected
- TabReselected
- TabUnselected

### Adding Tabs to the ActionBar

The ActionBar is native to Android 3.0 (API level 11) and higher and is 
available to any Xamarin.Android application that targets this API as a 
minimum. 

The following steps illustrate how to add ActionBar tabs to an Android 
Activity: 

1. In the `OnCreate` method of an Activity &ndash; *before initializing
   any UI widgets* &ndash; an application must set the `NavigationMode`
   on the `ActionBar` to `ActionBar.NavigationModeTabs` as shown in
   this code snippet:

   ```csharp
   ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
   SetContentView(Resource.Layout.Main);
   ```

2. Create a new tab using `ActionBar.NewTab()`.

3. Assign event handlers or provide a custom `ActionBar.ITabListener`
   implementation that will respond to the events that are raised when
   the user interacts with the ActionBar tabs.

4. Add the tab that was created in the previous step to the
   `ActionBar`.

The following code is one example of using these steps to add tabs to 
an application that uses event handlers to respond to state changes: 

```csharp
protected override void OnCreate(Bundle bundle)
{
    ActionBar.NavigationMode = ActionBarNavigationMode.Tabs;
    SetContentView(Resource.Layout.Main);

    ActionBar.Tab tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab1_text));
    tab.SetIcon(Resource.Drawable.tab1_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);

    tab = ActionBar.NewTab();
    tab.SetText(Resources.GetString(Resource.String.tab2_text));
    tab.SetIcon(Resource.Drawable.tab2_icon);
    tab.TabSelected += (sender, args) => {
                           // Do something when tab is selected
                       };
    ActionBar.AddTab(tab);
}
```

#### Event Handlers vs ActionBar.ITabListener

Applications should use event handlers and `ActionBar.ITabListener` for 
different scenarios. Event handlers do offer a certain amount of 
syntactic convenience; they save you from having to create a class and 
implement `ActionBar.ITabListener`. This convenience does come at a 
cost &ndash; Xamarin.Android performs this transformation for you, creating 
one class and implementing `ActionBar.ITabListener` for you. This is 
fine when an application has a limited number of tabs. 

When dealing with many tabs, or sharing common functionality between 
ActionBar tabs, it can be more efficient in terms of memory and 
performance to create a custom class that implements 
`ActionBar.ITabListener`, and sharing a single instance of the class. 
This will reduce the number of GREF's that a Xamarin.Android 
application is using. 

### Backwards Compatibility for Older Devices

The
[Android Support Library v7 AppCompat](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
back ports ActionBar tabs to Android 2.1 (API level 7). Tabs are
accessible in a Xamarin.Android application once this component has
been added to the project.

To use the ActionBar, an activity must subclass `ActionBarActivity` and
use the AppCompat theme as shown in the following code snippet:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity: ActionBarActivity
```

An Activity may obtain a reference to its ActionBar from the
`ActionBarActivity.SupportingActionBar` property. The following code
snippet illustrates an example of setting up the ActionBar in an
Activity:

```csharp
[Activity(Label = "@string/app_name", Theme = "@style/Theme.AppCompat", MainLauncher = true, Icon = "@drawable/ic_launcher")]
public class MainActivity : ActionBarActivity, ActionBar.ITabListener
{
    static readonly string Tag = "ActionBarTabsSupport";

    public void OnTabReselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Optionally refresh/update the displayed tab.
        Log.Debug(Tag, "The tab {0} was re-selected.", tab.Text);
    }

    public void OnTabSelected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Display the fragment the user should see
        Log.Debug(Tag, "The tab {0} has been selected.", tab.Text);
    }

    public void OnTabUnselected(ActionBar.Tab tab, FragmentTransaction ft)
    {
        // Save any state in the displayed fragment.
        Log.Debug(Tag, "The tab {0} as been unselected.", tab.Text);
    }

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SupportActionBar.NavigationMode = ActionBar.NavigationModeTabs;
        SetContentView(Resource.Layout.Main);
    }

    void AddTabToActionBar(int labelResourceId, int iconResourceId)
    {
        ActionBar.Tab tab = SupportActionBar.NewTab()
                                            .SetText(labelResourceId)
                                            .SetIcon(iconResourceId)
                                            .SetTabListener(this);
        SupportActionBar.AddTab(tab);
    }
}
```

## Summary

In this guide we discussed how to create a tabbed user interface in a 
Xamarin.Android using the ActionBar. We covered how to add tabs to the 
ActionBar and how an Activity can interact with tab events via the 
`ActionBar.ITabListener` interface. We also saw how the Android Support 
Library v7 AppCompat package backports the ActionBar tabs to older 
versions of Android. 

## Related Links

- [ActionBarTabs (sample)](/samples/xamarin/monodroid-samples/userinterface-actionbartabs)
- [Toolbar](~/android/user-interface/controls/tool-bar/index.md)
- [Fragments](~/android/platform/fragments/index.md)
- [ActionBar](https://developer.android.com/guide/topics/ui/actionbar.html)
- [ActionBarActivity](https://developer.android.com/reference/android/app/ActionBar)
- [Action Bar Pattern](https://developer.android.com/design/patterns/actionbar.html)
- [Android v7 AppCompat](https://developer.android.com/tools/support-library/features.html#v7-appcompat)
- [Xamarin.Android Support Library v7 AppCompat NuGet Package](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)