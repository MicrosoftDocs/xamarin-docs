---
title: "Keyboard Navigation"
description: "Rather than using the default tab sequence, it's sometimes necessary to tune your UI by specifying the tab sequence with a combination of the TabIndex and IsTapStop properties."
ms.prod: xamarin
ms.assetid: 8be8f498-558a-4894-a01f-91a0d3ef927e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
---

# Keyboard Navigation in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)

Some users can have difficulty using applications that don't provide appropriate keyboard access. Specifying a tab order for controls enables keyboard navigation and prepares application pages to receive input in a particular order.

By default, the tab order of controls is the same order in which they are listed in XAML, or programmatically added to a child collection. This order is the order in which the controls will be navigated through with a keyboard, and often this default order is the best order. However, the default order is not always the same as the expected order, as shown in the following XAML code example:

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname" />
</Grid>
```

The following screenshot shows the default tab order for this code example:

![](keyboard-images/default-tab-order.png "Default Row-based Tab Order")

The tab order here is row-based, and is the order the controls are listed in the XAML. Therefore, pressing the Tab key navigates through forename [`Entry`](xref:Xamarin.Forms.Entry) instances, followed by surname `Entry` instances. However, a more intuitive experience would be to use a column-first tab navigation, so that pressing the Tab key navigates through forename-surname pairs. This can be achieved by specifying the tab order of the input controls.

> [!NOTE]
> On the Universal Windows Platform, keyboard shortcuts can be defined that provide an intuitive way for users to quickly navigate and interact with the application's visible UI through a keyboard instead of via touch or a mouse. For more information, see [Setting VisualElement Access Keys](~/xamarin-forms/platform/windows/visualelement-access-keys.md).

## Setting the tab order

The `VisualElement.TabIndex` property is used to indicate the order in which [`VisualElement`](xref:Xamarin.Forms.VisualElement) instances receive focus when the user navigates through controls by pressing the Tab key. The default value of the property is 0, and it can be set to any `int` value.

The following rules apply when using the default tab order, or setting the `TabIndex` property:

 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) instances with a `TabIndex` equal to 0 are added to the tab order based on their declaration order in XAML or child collections.
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) instances with a `TabIndex` greater than 0 are added to the tab order based on their `TabIndex` value.
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) instances with a `TabIndex` less than 0 are added to the tab order and appear before any zero value.
 - Conflicts on a `TabIndex` are resolved by declaration order.

After defining a tab order, pressing the Tab key will cycle the focus through controls in ascending `TabIndex` order, wrapping around to the beginning once the final control is reached.

The following XAML example shows the `TabIndex` property set on input controls to enable column-first tab navigation:

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="1" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="3" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="2" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="4" />
</Grid>
```

The following screenshot shows the tab order for this code example:

![](keyboard-images/correct-tab-order.png "Column-based Tab Order")

The tab order here is column-based. Therefore, pressing the Tab key navigates through forename-surname [`Entry`](xref:Xamarin.Forms.Entry) pairs.

## Excluding controls from the tab order

In addition to setting the tab order of controls, it may be necessary to exclude controls from the tab order. One way of achieving this is by setting the [`IsEnabled`](xref:Xamarin.Forms.VisualElement) property of controls to `false`, because disabled controls are excluded from the tab order.

However, it may be necessary to exclude controls from the tab order even when they aren't disabled. This can be achieved with the `VisualElement.IsTapStop` property, which indicates whether a [`VisualElement`](xref:Xamarin.Forms.VisualElement) is included in tab navigation. Its default value is `true`, and when its value is `false` the control is ignored by the tab-navigation infrastructure, irrespective if a `TabIndex` is set.

## Supported controls

The `TabIndex` and `IsTapStop` properties are supported on the following controls, which accept keyboard input on one or more platforms:

- [`Button`](xref:Xamarin.Forms.Button)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`SearchBar`](xref:Xamarin.Forms.SearchBar)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)
- [`Switch`](xref:Xamarin.Forms.Switch)
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)

> [!NOTE]
> Each of these controls isn't tab focusable on every platform.

## Related Links

- [Accessibility (sample)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
