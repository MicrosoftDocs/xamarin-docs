---
title: "ListView Performance"
description: "Although ListView is a powerful view for displaying data, it has some limitations. This article explains how to ensure great performance with a Xamarin.Forms ListView in an application."
ms.service: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# ListView performance

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)

When writing mobile applications, performance matters. Users have come to expect smooth scrolling and fast load times. Failing to meet your users' expectations will cost you ratings in the application store, or in the case of a line-of-business application, cost your organization time and money.

The Xamarin.Forms [`ListView`](xref:Xamarin.Forms.ListView) is a powerful view for displaying data, but it has some limitations. Scrolling performance can suffer when using custom cells, especially when they contain deeply nested view hierarchies or use certain layouts that require complex measurement. Fortunately, there are techniques you can use to avoid poor performance.

## Caching strategy

ListViews are often used to display much more data than fits onscreen. For example, a music app might have a library of songs with thousands of entries. Creating an item for every entry would waste valuable memory and perform poorly. Creating and destroying rows constantly would require the application to instantiate and cleanup objects constantly, which would also perform poorly.

To conserve memory, the native [`ListView`](xref:Xamarin.Forms.ListView) equivalents for each platform have built-in features for reusing rows. Only the cells visible on screen are loaded in memory and the **content** is loaded into existing cells. This pattern prevents the application from instantiating thousands of objects, saving time and memory.

Xamarin.Forms permits [`ListView`](xref:Xamarin.Forms.ListView) cell reuse through the [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) enumeration, which has the following values:

```csharp
public enum ListViewCachingStrategy
{
    RetainElement,   // the default value
    RecycleElement,
    RecycleElementAndDataTemplate
}
```

> [!NOTE]
> The Universal Windows Platform (UWP) ignores the [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) caching strategy, because it always uses caching to improve performance. Therefore, by default it behaves as if the [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) caching strategy is applied.

### RetainElement

The [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) caching strategy specifies that the [`ListView`](xref:Xamarin.Forms.ListView) will generate a cell for each item in the list, and is the default `ListView` behavior. It should be used in the following circumstances:

- Each cell has a large number of bindings (20-30+).
- The cell template changes frequently.
- Testing reveals that the `RecycleElement` caching strategy results in a reduced execution speed.

It's important to recognize the consequences of the [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) caching strategy when working with custom cells. Any cell initialization code will need to run for each cell creation, which may be multiple times per second. In this circumstance, layout techniques that were fine on a page, like using multiple nested [`StackLayout`](xref:Xamarin.Forms.StackLayout) instances, become performance bottlenecks when they're set up and destroyed in real time as the user scrolls.

### RecycleElement

The [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) caching strategy specifies that the [`ListView`](xref:Xamarin.Forms.ListView) will attempt to minimize its memory footprint and execution speed by recycling list cells. This mode doesn't always offer a performance improvement, and testing should be performed to determine any improvements. However, it's the preferred choice, and should be used in the following circumstances:

- Each cell has a small to moderate number of bindings.
- Each cell's [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) defines all of the cell data.
- Each cell is largely similar, with the cell template unchanging.

During virtualization the cell will have its binding context updated, and so if an application uses this mode it must ensure that binding context updates are handled appropriately. All data about the cell must come from the binding context or consistency errors may occur. This problem can be avoided by using data binding to display cell data. Alternatively, cell data should be set in the `OnBindingContextChanged` override, rather than in the custom cell's constructor, as demonstrated in the following code example:

```csharp
public class CustomCell : ViewCell
{
    Image image = null;
    
    public CustomCell ()
    {
        image = new Image();
        View = image;
    }
    
    protected override void OnBindingContextChanged ()
    {
        base.OnBindingContextChanged ();
        
        var item = BindingContext as ImageItem;
        if (item != null) {
            image.Source = item.ImageUrl;
        }
    }
}
```

For more information, see [Binding Context Changes](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md#binding-context-changes).

On iOS and Android, if cells use custom renderers, they must ensure that property change notification is correctly implemented. When cells are reused their property values will change when the binding context is updated to that of an available cell, with `PropertyChanged` events being raised. For more information, see [Customizing a ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

#### RecycleElement with a DataTemplateSelector

When a [`ListView`](xref:Xamarin.Forms.ListView) uses a [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) to select a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), the [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) caching strategy does not cache `DataTemplate`s. Instead, a `DataTemplate` is selected for each item of data in the list.

> [!NOTE]
> The [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) caching strategy has a pre-requisite, introduced in Xamarin.Forms 2.4, that when a [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) is asked to select a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) that each `DataTemplate` must return the same [`ViewCell`](xref:Xamarin.Forms.ViewCell) type. For example, given a [`ListView`](xref:Xamarin.Forms.ListView) with a `DataTemplateSelector` that can return either `MyDataTemplateA` (where `MyDataTemplateA` returns a `ViewCell` of type `MyViewCellA`), or `MyDataTemplateB` (where `MyDataTemplateB` returns a `ViewCell` of type `MyViewCellB`), when `MyDataTemplateA` is returned it must return `MyViewCellA` or an exception will be thrown.

### RecycleElementAndDataTemplate

The [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) caching strategy builds on the [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) caching strategy by additionally ensuring that when a [`ListView`](xref:Xamarin.Forms.ListView) uses a [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) to select a [`DataTemplate`](xref:Xamarin.Forms.DataTemplate), `DataTemplate`s are cached by the type of item in the list. Therefore, `DataTemplate`s are selected once per item type, instead of once per item instance.

> [!NOTE]
> The [`RecycleElementAndDataTemplate`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElementAndDataTemplate) caching strategy has a pre-requisite that the `DataTemplate`s returned by the [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) must use the [`DataTemplate`](xref:Xamarin.Forms.DataTemplate.%23ctor(System.Type)) constructor that takes a `Type`.

### Set the caching strategy

The [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) enumeration value is specified with a [`ListView`](xref:Xamarin.Forms.ListView) constructor overload, as shown in the following code example:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

In XAML, set the `CachingStrategy` attribute as shown in the XAML below:

```xaml
<ListView CachingStrategy="RecycleElement">
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
              ...
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

This method has the same effect as setting the caching strategy argument in the constructor in C#.

#### Set the caching strategy in a subclassed ListView

Setting the `CachingStrategy` attribute from XAML on a subclassed [`ListView`](xref:Xamarin.Forms.ListView) will not produce the desired behavior, because there's no `CachingStrategy` property on `ListView`. In addition, if [XAMLC](~/xamarin-forms/xaml/xamlc.md) is enabled, the following error message will be produced: **No property, bindable property, or event found for 'CachingStrategy'**

The solution to this issue is to specify a constructor on the subclassed [`ListView`](xref:Xamarin.Forms.ListView) that accepts a [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) parameter and passes it into the base class:

```csharp
public class CustomListView : ListView
{
    public CustomListView (ListViewCachingStrategy strategy) : base (strategy)
    {
    }
    ...
}
```

Then the [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) enumeration value can be specified from XAML by using the `x:Arguments` syntax:

```xaml
<local:CustomListView>
    <x:Arguments>
        <ListViewCachingStrategy>RecycleElement</ListViewCachingStrategy>
    </x:Arguments>
</local:CustomListView>
```

## ListView performance suggestions

There are many techniques for improving the performance of a `ListView`. The following suggestions may improve the performance of your ListView

- Bind the `ItemsSource` property to an `IList<T>` collection instead of an `IEnumerable<T>` collection, because `IEnumerable<T>` collections don't support random access.
- Use the built-in cells (like  `TextCell` / `SwitchCell` ) instead of `ViewCell` whenever you can.
- Use fewer elements. For example, consider using a single `FormattedString` label instead of multiple labels.
- Replace the `ListView` with a `TableView` when displaying non-homogenous data – that is, data of different types.
- Limit the use of the [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) method. If overused, it will degrade performance.
- On Android, avoid setting a `ListView`'s row separator visibility or color after it has been instantiated, as it results in a large performance penalty.
- Avoid changing the cell layout based on the [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext). Changing layout incurs large measurement and initialization costs.
- Avoid deeply nested layout hierarchies. Use  `AbsoluteLayout` or  `Grid` to help reduce nesting.
- Avoid specific `LayoutOptions` other than  `Fill` (`Fill` is the cheapest to compute).
- Avoid placing a `ListView` inside a `ScrollView` for the following reasons:
  - The `ListView` implements its own scrolling.
  - The `ListView` will not receive any gestures, as they will be handled by the parent `ScrollView`.
  - The `ListView` can present a customized header and footer that scrolls with the elements of the list, potentially offering the functionality that the `ScrollView` was used for. For more information, see [Headers and Footers](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#headers-and-footers).
- Consider a custom renderer if you need a specific, complex design presented in your cells.

`AbsoluteLayout` has the potential to perform layouts without a single measure call, making it highly performant. If `AbsoluteLayout` cannot be used, consider [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout). If using `RelativeLayout`, passing Constraints directly will be considerably faster than using the expression API. This method is faster because the expression API uses JIT, and on iOS the tree has to be interpreted, which is slower. The expression API is suitable for page layouts where it only required on initial layout and rotation, but in `ListView`, where it's run constantly during scrolling, it hurts performance.

Building a custom renderer for a [`ListView`](xref:Xamarin.Forms.ListView) or its cells is one approach to reducing the effect of layout calculations on scrolling performance. For more information, see [Customizing a ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) and [Customizing a ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).

## Related links

- [Custom Renderer View (sample)](/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative)
- [Custom Renderer ViewCell (sample)](/samples/xamarin/xamarin-forms-samples/customrenderers-viewcell)
- [ListViewCachingStrategy](xref:Xamarin.Forms.ListViewCachingStrategy)