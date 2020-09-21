---
title: "Xamarin.Forms dual-screen triggers"
description: "This article explains how to use Xamarin.Forms dual-screen triggers to respond to user interface changes with XAML."
ms.prod: xamarin
ms.assetid: 2181715D-3995-4E71-9A21-6B892F0B3B59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms dual-screen triggers

The [`Xamarin.Forms.DualScreen`](xref:Xamarin.Forms.DualScreen) namespace includes two state triggers:

- [`SpanModeStateTrigger`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger) triggers a [`VisualState`](xref:Xamarin.Forms.VisualState) change when the view mode of the attached layout changes.
- `WindowSpanModeStateTrigger` triggers a [`VisualState`](xref:Xamarin.Forms.VisualState) change when the view mode of the window changes.

For more information about state triggers, see [State triggers](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers).

## Span mode state trigger

A [`SpanModeStateTrigger`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger) triggers a [`VisualState`](xref:Xamarin.Forms.VisualState) change when the span mode of the attached layout changes. This trigger has a single bindable property:

- [`SpanMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode), of type [`TwoPaneViewMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode), which indicates the span mode to which the [`VisualState`](xref:Xamarin.Forms.VisualState) should be applied.

> [!NOTE]
> The [`SpanModeStateTrigger`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger) derives from the [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) class and can therefore attach an event handler to the [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) event.

The following XAML example shows a [`Grid`](xref:Xamarin.Forms.Grid) that includes `SpanModeStateTrigger` objects:

```xaml
<Grid>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="GridSingle">
                <VisualState.StateTriggers>
                    <dualScreen:SpanModeStateTrigger SpanMode="SinglePane"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Green" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="GridWide">
                <VisualState.StateTriggers>
                    <dualScreen:SpanModeStateTrigger SpanMode="Wide" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Red" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="GridTall">
                <VisualState.StateTriggers>
                    <dualScreen:SpanModeStateTrigger SpanMode="Tall" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Purple" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    ...
</Grid>
```

In this example, visual states are set on a [`Grid`](xref:Xamarin.Forms.Grid) object. The background color of the `Grid` is green when only one pane is shown, is red when panes are shown side by side, and is purple when panes are shown top-bottom.

## Window span mode state trigger

A `WindowSpanModeStateTrigger` triggers a [`VisualState`](xref:Xamarin.Forms.VisualState) change when the span mode of the window changes. This trigger has a single bindable property:

- [`SpanMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode), of type [`TwoPaneViewMode`](xref:Xamarin.Forms.DualScreen.SpanModeStateTrigger.SpanMode), which indicates the span mode to which the [`VisualState`](xref:Xamarin.Forms.VisualState) should be applied.

> [!NOTE]
> The `WindowSpanModeStateTrigger` derives from the [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) class and can therefore attach an event handler to the [`IsActiveChanged`](xref:Xamarin.Forms.StateTriggerBase.IsActiveChanged) event.

The following XAML example shows a [`Grid`](xref:Xamarin.Forms.Grid) that includes `WindowSpanModeStateTrigger` objects:

```xaml
<Grid>
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup>
            <VisualState x:Name="NotSpanned">
                <VisualState.StateTriggers>
                    <dualScreen:WindowSpanModeStateTrigger SpanMode="SinglePane"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Red" />
                </VisualState.Setters>
            </VisualState>
            <VisualState x:Name="Spanned">
                <VisualState.StateTriggers>
                    <dualScreen:WindowSpanModeStateTrigger SpanMode="Wide" />
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Green" />
                </VisualState.Setters>
            </VisualState>
                <VisualState x:Name="Tall">
                    <VisualState.StateTriggers>
                        <dualScreen:WindowSpanModeStateTrigger SpanMode="Tall" />
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Property="BackgroundColor" Value="Yellow" />
                    </VisualState.Setters>
                </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
    ...
</Grid>    
```

In this example, visual states are set on a [`Grid`](xref:Xamarin.Forms.Grid) object. The background color of the `Grid` is red when only one pane is shown, is green when panes are shown side by side, and is yellow when panes are shown top-bottom.

## Related links

- [Xamarin.Forms Triggers](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
