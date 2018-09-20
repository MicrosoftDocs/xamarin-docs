---
title: "Windows Platform-Specifics"
description: "Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article demonstrates how to consume the Windows platform-specifics that are built into Xamarin.Forms."
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
---

# Windows Platform-Specifics

_Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects. This article demonstrates how to consume the Windows platform-specifics that are built into Xamarin.Forms._

## VisualElements

On the Universal Windows Platform, the following platform-specific functionality is provided for Xamarin.Forms views, pages, and layouts:

- Setting an access key for a [`VisualElement`](xref:Xamarin.Forms.VisualElement). For more information, see [Setting VisualElement Access Keys](#visualelement-accesskeys).
- Disabling legacy color mode on a supported [`VisualElement`](xref:Xamarin.Forms.VisualElement). For more information, see [Disabling Legacy Color Mode](#legacy-color-mode).

<a name="visualelement-accesskeys" />

### Setting VisualElement Access Keys

Access keys are keyboard shortcuts that improve the usability and accessibility of apps on the Universal Windows Platform by providing an intuitive way for users to quickly navigate and interact with the app's visible UI through a keyboard instead of via touch or a mouse. They are combinations of the Alt key and one or more alphanumeric keys, typically pressed sequentially. Keyboard shortcuts are automatically supported for access keys that use a single alphanumeric character.

Access key tips are floating badges displayed next to controls that include access keys. Each access key tip contains the alphanumeric keys that activate the associated control. When a user presses the Alt key, the access key tips are displayed.

This platform-specific is used to specify an access key for a [`VisualElement`](xref:Xamarin.Forms.VisualElement). It's consumed in XAML by setting the [`VisualElement.AccessKey`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyProperty) attached property to an alphanumeric value, and by optionally setting the [`VisualElement.AccessKeyPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyPlacementProperty) attached property to a value of the [`AccessKeyPlacement`](xref:Xamarin.Forms.AccessKeyPlacement) enumeration, the [`VisualElement.AccessKeyHorizontalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyHorizontalOffsetProperty) attached property to a `double`, and the [`VisualElement.AccessKeyVerticalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.AccessKeyVerticalOffsetProperty) attached property to a `double`:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <ContentPage Title="Page 1"
                 windows:VisualElement.AccessKey="1">
        <StackLayout Margin="20">
            ...
            <Switch windows:VisualElement.AccessKey="A" />
            <Entry Placeholder="Enter text here"
                   windows:VisualElement.AccessKey="B" />
            ...
            <Button Text="Access key F, placement top with offsets"
                    Margin="20"
                    Clicked="OnButtonClicked"
                    windows:VisualElement.AccessKey="F"
                    windows:VisualElement.AccessKeyPlacement="Top"
                    windows:VisualElement.AccessKeyHorizontalOffset="20"
                    windows:VisualElement.AccessKeyVerticalOffset="20" />
            ...
        </StackLayout>
    </ContentPage>
    ...
</TabbedPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var page = new ContentPage { Title = "Page 1" };
page.On<Windows>().SetAccessKey("1");

var switchView = new Switch();
switchView.On<Windows>().SetAccessKey("A");
var entry = new Entry { Placeholder = "Enter text here" };
entry.On<Windows>().SetAccessKey("B");
...

var button4 = new Button { Text = "Access key F, placement top with offsets", Margin = new Thickness(20) };
button4.Clicked += OnButtonClicked;
button4.On<Windows>()
    .SetAccessKey("F")
    .SetAccessKeyPlacement(AccessKeyPlacement.Top)
    .SetAccessKeyHorizontalOffset(20)
    .SetAccessKeyVerticalOffset(20);
...
```

The `VisualElement.On<Windows>` method specifies that this platform-specific will only run on the Universal Windows Platform. The [`VisualElement.SetAccessKey`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.String)) method, in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace, is used to set the access key value for the `VisualElement`. The [`VisualElement.SetAccessKeyPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},Xamarin.Forms.AccessKeyPlacement)) method, optionally specifies the position to use for displaying the access key tip, with the [`AccessKeyPlacement`](xref:Xamarin.Forms.AccessKeyPlacement) enumeration providing the following possible values:

- [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) – indicates that the access key tip placement will be determined by the operating system.
- [`Top`](xref:Xamarin.Forms.AccessKeyPlacement.Top) – indicates that the access key tip will appear above the top edge of the `VisualElement`.
- [`Bottom`](xref:Xamarin.Forms.AccessKeyPlacement.Bottom) – indicates that the access key tip will appear below the lower edge of the `VisualElement`.
- [`Right`](xref:Xamarin.Forms.AccessKeyPlacement.Right) – indicates that the access key tip will appear to the right of the right edge of the `VisualElement`.
- [`Left`](xref:Xamarin.Forms.AccessKeyPlacement.Left) – indicates that the access key tip will appear to the left of the left edge of the `VisualElement`.
- [`Center`](xref:Xamarin.Forms.AccessKeyPlacement.Center) – indicates that the access key tip will appear overlaid on the center of the `VisualElement`.

> [!NOTE]
> Typically, the [`Auto`](xref:Xamarin.Forms.AccessKeyPlacement.Auto) key tip placement is sufficient, which includes support for adaptive user interfaces.

The [`VisualElement.SetAccessKeyHorizontalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) and [`VisualElement.SetAccessKeyVerticalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Double)) methods can be used for more granular control of the access key tip location. The argument to the `SetAccessKeyHorizontalOffset` method indicates how far to move the access key tip left or right, and the argument to the `SetAccessKeyVerticalOffset` method indicates how far to move the access key tip up or down.

>[!NOTE]
> Access key tip offsets can't be set when the access key placement is set `Auto`.

In addition, the [`GetAccessKey`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKey(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [`GetAccessKeyPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), [`GetAccessKeyHorizontalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyHorizontalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})), and [`GetAccessKeyVerticalOffset`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetAccessKeyVerticalOffset(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) methods can be used to retrieve an access key value and it's location.

The result is that access key tips can be displayed next to any [`VisualElement`](xref:Xamarin.Forms.VisualElement) instances that define access keys, by pressing the Alt key:

![VisualElement access keys platform-specific](windows-images/visualelement-accesskeys.png "VisualElement access keys platform-specific")

When a user activates an access key, by pressing the Alt key followed by the access key, the default action for the `VisualElement` will be executed. For example, when a user activates the access key on a [`Switch`](xref:Xamarin.Forms.Switch), the `Switch` is toggled. When a user activates the access key on an [`Entry`](xref:Xamarin.Forms.Entry), the `Entry` gains focus. When a user activates the access key on a [`Button`](xref:Xamarin.Forms.Button), the event handler for the [`Clicked`](xref:Xamarin.Forms.Button.Clicked) event is executed.

For more information about access keys, see [Access keys](/windows/uwp/design/input/access-keys#key-tip-positioning).

<a name="legacy-color-mode" />

### Disabling Legacy Color Mode

Some of the Xamarin.Forms views feature a legacy color mode. In this mode, when the [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) property of the view is set to `false`, the view will override the colors set by the user with the default native colors for the disabled state. For backwards compatibility, this legacy color mode remains the default behavior for supported views.

This platform-specific disables this legacy color mode, so that colors set on a view by the user remain even when the view is disabled. It's consumed in XAML by setting the [`VisualElement.IsLegacyColorModeEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) attached property to `false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

The `VisualElement.On<Windows>` method specifies that this platform-specific will only run on Windows. The [`VisualElement.SetIsLegacyColorModeEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) method, in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace, is used to control whether the legacy color mode is disabled. In addition, the [`VisualElement.GetIsLegacyColorModeEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) method can be used to return whether the legacy color mode is disabled.

The result is that the legacy color mode can be disabled, so that colors set on a view by the user remain even when the view is disabled:

![](windows-images/legacy-color-mode-disabled.png "Legacy color mode disabled")

> [!NOTE]
> When setting a [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) on a view, the legacy color mode is completely ignored. For more information about visual states, see [The Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## Views

On the Universal Windows Platform (UWP), the following platform-specific functionality is provided for Xamarin.Forms views:

- Detecting reading order from text content in [`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), and [`Label`](xref:Xamarin.Forms.Label) instances. For more information, see [Detecting Reading Order from Content](#inputview-readingorder).
- Enabling tap gesture support in a [`ListView`](xref:Xamarin.Forms.ListView). For more information, see [Enabling Tap Gesture Support in a ListView](#listview-selectionmode).
- Enabling a [`SearchBar`](xref:Xamarin.Forms.SearchBar) to interact with the spell check engine. For more information, see [Enabling SearchBar Spell Check](#searchbar-spellcheck).
- Enabling a [`WebView`](xref:Xamarin.Forms.WebView) to display JavaScript alerts in a UWP message dialog. For more information, see [Displaying JavaScript Alerts](#webview-javascript-alert).

<a name="inputview-readingorder" />

### Detecting Reading Order from Content

This platform-specific enables the reading order (left-to-right or right-to-left) of bidirectional text in [`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), and [`Label`](xref:Xamarin.Forms.Label) instances to be detected dynamically. It's consumed in XAML by setting the [`InputView.DetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (for `Entry` and `Editor` instances) or [`Label.DetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) attached property to a `boolean` value:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

The `Editor.On<Windows>` method specifies that this platform-specific will only run on the Universal Windows Platform. The [`InputView.SetDetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) method, in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace, is used to control whether the reading order is detected from the content in the [`InputView`](xref:Xamarin.Forms.InputView). In addition, the `InputView.SetDetectReadingOrderFromContent` method can be used to toggle whether the reading order is detected from the content by calling the [`InputView.GetDetectReadingOrderFromContent`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) method to return the current value:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

The result is that [`Entry`](xref:Xamarin.Forms.Entry), [`Editor`](xref:Xamarin.Forms.Editor), and [`Label`](xref:Xamarin.Forms.Label) instances can have the reading order of their content detected dynamically:

[![InputView detecting reading order from content platform-specific](windows-images/editor-readingorder.png "InputView detecting reading order from content platform-specific")](windows-images/editor-readingorder-large.png#lightbox "InputView detecting reading order from content platform-specific")

> [!NOTE]
> Unlike setting the [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) property, the logic for views that detect the reading order from their text content will not affect the alignment of text within the view. Instead, it adjusts the order in which blocks of bidirectional text are laid out.

<a name="listview-selectionmode" />

### Enabling Tap Gesture Support in a ListView

On the Universal Windows Platform, by default the Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) uses the native `ItemClick` event to respond to interaction, rather than the native `Tapped` event. This provides accessibility functionality so that the Windows Narrator and the keyboard can interact with the `ListView`. However, it also renders any tap gestures inside the `ListView` inoperable.

This platform-specific controls whether items in a [`ListView`](xref:Xamarin.Forms.ListView) can respond to tap gestures, and hence whether the native `ListView` fires the `ItemClick` or `Tapped` event. It's consumed in XAML by setting the [`ListView.SelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) attached property to a value of the [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) enumeration:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

The `ListView.On<Windows>` method specifies that this platform-specific will only run on the Universal Windows Platform. The [`ListView.SetSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) method, in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace, is used to control whether items in a [`ListView`](xref:Xamarin.Forms.ListView) can respond to tap gestures, with the [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) enumeration providing two possible values:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – indicates that the `ListView` will fire the native `ItemClick` event to handle interaction, and hence provide accessibility functionality. Therefore, the Windows Narrator and the keyboard can interact with the `ListView`. However, items in the `ListView` can't respond to tap gestures. This is the default behavior for `ListView` instances on the Universal Windows Platform.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – indicates that the `ListView` will fire the native `Tapped` event to handle interaction. Therefore, items in the `ListView` can respond to tap gestures. However, there's no accessibility functionality and hence the Windows Narrator and the keyboard can't interact with the `ListView`.

> [!NOTE]
> The `Accessible` and `Inaccessible` selection modes are mutually exclusive, and you will need to choose between an accessible [`ListView`](xref:Xamarin.Forms.ListView) or a `ListView` that can respond to tap gestures.

In addition, the [`GetSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) method can be used to return the current [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

The result is that a specified [`ListViewSelectionMode`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) is applied to the [`ListView`](xref:Xamarin.Forms.ListView), which controls whether items in the `ListView` can respond to tap gestures, and hence whether the native `ListView` fires the `ItemClick` or `Tapped` event.

<a name="searchbar-spellcheck" />

### Enabling SearchBar Spell Check

This platform-specific enables a [`SearchBar`](xref:Xamarin.Forms.SearchBar) to interact with the spell check engine. It's consumed in XAML by setting the [`SearchBar.IsSpellCheckEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) attached property to a `boolean` value:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

The `SearchBar.On<Windows>` method specifies that this platform-specific will only run on the Universal Windows Platform. The [`SearchBar.SetIsSpellCheckEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) method, in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace, turns the spell checker on and off. In addition, the `SearchBar.SetIsSpellCheckEnabled` method can be used to toggle the spell checker by calling the [`SearchBar.GetIsSpellCheckEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) method to return whether the spell checker is enabled:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

The result is that text entered into the [`SearchBar`](xref:Xamarin.Forms.SearchBar) can be spell checked, with incorrect spellings being indicated to the user:

![SearchBar spell check platform-specific](windows-images/searchbar-spellcheck.png "SearchBar spell check platform-specific")

> [!NOTE]
> The `SearchBar` class in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace also has [`EnableSpellCheck`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) and [`DisableSpellCheck`](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) methods that can be used to enable and disable the spell checker on the `SearchBar`, respectively.

<a name="webview-javascript-alert" />

### Displaying JavaScript Alerts

This platform-specific enables a [`WebView`](xref:Xamarin.Forms.WebView) to display JavaScript alerts in a UWP message dialog. It's consumed in XAML by setting the [`WebView.IsJavaScriptAlertEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) attached property to a `boolean` value:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

The `WebView.On<Windows>` method specifies that this platform-specific will only run on the Universal Windows Platform. The [`WebView.SetIsJavaScriptAlertEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) method, in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace, is used to control whether JavaScript alerts are enabled. In addition, the `WebView.SetIsJavaScriptAlertEnabled` method can be used to toggle JavaScript alerts by calling the [`IsJavaScriptAlertEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) method to return whether they are enabled:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

The result is that JavaScript alerts can be displayed in a UWP message dialog:

![WebView JavaScript alert platform-specific](windows-images/webview-javascript-alert.png "WebView JavaScript alert platform-specific")

## Pages

On the Universal Windows Platform, the following platform-specific functionality is provided for Xamarin.Forms pages:

- Collapsing the [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) navigation bar. For more information, see [Collapsing a MasterDetailPage Navigation Bar](#collapsable_navigation_bar).
- Setting toolbar placement options. For more information, see [Changing the Page Toolbar Placement](#toolbar_placement).
- Enabling page icons to be displayed on a [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) toolbar. For more information, see [Enabling Icons on a TabbedPage](#tabbedpage-icons).

<a name="collapsable_navigation_bar" />

### Collapsing a MasterDetailPage Navigation Bar

This platform-specific is used to collapse the navigation bar on a [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage), and is consumed in XAML by setting the [`MasterDetailPage.CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty) and [`MasterDetailPage.CollapsedPaneWidth`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty) attached properties:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

The `MasterDetailPage.On<Windows>` method specifies that this platform-specific will only run on Windows. The [`Page.SetCollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle)) method, in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace, is used to specify the collapse style, with the [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) enumeration providing two values: [`Full`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) and [`Partial`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial). The [`MasterDetailPage.CollapsedPaneWidth`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage},System.Double)) method is used to specify the width of a partially collapsed navigation bar.

The result is that a specified [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) is applied to the [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) instance, with the width also being specified:

[![](windows-images/collapsed-navigation-bar.png "Collapsed Navigation Bar Platform-Specific")](windows-images/collapsed-navigation-bar-large.png#lightbox "Collapsed Navigation Bar Platform-Specific")

<a name="toolbar_placement" />

### Changing the Page Toolbar Placement

This platform-specific is used to change the placement of a toolbar on a [`Page`](xref:Xamarin.Forms.Page), and is consumed in XAML by setting the [`Page.ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty) attached property to a value of the [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) enumeration:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

The `Page.On<Windows>` method specifies that this platform-specific will only run on Windows. The [`Page.SetToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement)) method, in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace, is used to set the toolbar placement, with the [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) enumeration providing three values: [`Default`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default), [`Top`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top), and [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom).

The result is that the specified toolbar placement is applied to the [`Page`](xref:Xamarin.Forms.Page) instance:

[![](windows-images/toolbar-placement.png "Toolbar Placement Platform-Specific")](windows-images/toolbar-placement-large.png#lightbox "Toolbar Placement Platform-Specific")

<a name="tabbedpage-icons" />

### Enabling Icons on a TabbedPage

This platform-specific enables page icons to be displayed on a [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) toolbar, and provides the ability to optionally specify the icon size. It's consumed in XAML by setting the [`TabbedPage.HeaderIconsEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsEnabledProperty) attached property to `true`, and by optionally setting the [`TabbedPage.HeaderIconsSize`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.HeaderIconsSizeProperty) attached property to a [`Size`](xref:Xamarin.Forms.Size) value:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:TabbedPage.HeaderIconsEnabled="true">
    <windows:TabbedPage.HeaderIconsSize>
        <Size>
            <x:Arguments>
                <x:Double>24</x:Double>
                <x:Double>24</x:Double>
            </x:Arguments>
        </Size>
    </windows:TabbedPage.HeaderIconsSize>
    <ContentPage Title="Todo" Icon="todo.png">
        ...
    </ContentPage>
    <ContentPage Title="Reminders" Icon="reminders.png">
        ...
    </ContentPage>
    <ContentPage Title="Contacts" Icon="contacts.png">
        ...
    </ContentPage>
</TabbedPage>
```

Alternatively, it can be consumed from C# using the fluent API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

public class WindowsTabbedPageIconsCS : Xamarin.Forms.TabbedPage
{
  public WindowsTabbedPageIconsCS()
	{
    On<Windows>().SetHeaderIconsEnabled(true);
    On<Windows>().SetHeaderIconsSize(new Size(24, 24));

    Children.Add(new ContentPage { Title = "Todo", Icon = "todo.png" });
    Children.Add(new ContentPage { Title = "Reminders", Icon = "reminders.png" });
    Children.Add(new ContentPage { Title = "Contacts", Icon = "contacts.png" });
  }
}
```

The `TabbedPage.On<Windows>` method specifies that this platform-specific will only run on the Universal Windows Platform. The [`TabbedPage.SetHeaderIconsEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},System.Boolean)) method, in the [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) namespace, is used to turn header icons on or off. The [`TabbedPage.SetHeaderIconsSize`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.SetHeaderIconsSize(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.TabbedPage},Xamarin.Forms.Size)) method optionally specifies the header icon size with a [`Size`](xref:Xamarin.Forms.Size) value.

In addition, the `TabbedPage` class in the `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` namespace also has a [`EnableHeaderIcons`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.EnableHeaderIcons*) method that enables header icons, a [`DisableHeaderIcons`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.DisableHeaderIcons*) method that disables header icons, and a [`IsHeaderIconsEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.TabbedPage.IsHeaderIconsEnabled*) method that returns a `boolean` value that indicates whether header icons are enabled.

The result is that page icons can be displayed on a [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) toolbar, with the icon size being optionally set to a desired size:

![TabbedPage icons enabled platform-specific](windows-images/tabbedpage-icons.png "TabbedPage icons enabled platform-specific")

## Summary

This article demonstrated how to consume the Windows platform-specifics that are built into Xamarin.Forms. Platform-specifics allow you to consume functionality that's only available on a specific platform, without implementing custom renderers or effects.

## Related Links

- [Creating Platform-Specifics](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (sample)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
