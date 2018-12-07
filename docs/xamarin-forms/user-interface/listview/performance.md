---
title: "ListView Performance"
description: "Although ListView is a powerful view for displaying data, it has some limitations. This article explains how to ensure great performance with a Xamarin.Forms ListView in an application."
ms.prod: xamarin
ms.assetid: 1B085639-652C-4862-86EB-5D55D32B9395
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2017
---

# ListView Performance

[![Download Sample](~/media/shared/download.png) Download the sample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)

When writing mobile applications, performance matters. Users have come to expect smooth scrolling and fast load times. Failing to meet your users' expectations will cost you ratings in the application store, or in the case of a line-of-business application, cost your organization time and money.

Although [`ListView`](xref:Xamarin.Forms.ListView) is a powerful view for displaying data, it has some limitations. Scrolling performance can suffer when using custom cells, especially when they contain deeply nested view hierarchies or use certain layouts that require a lot of measurement. Fortunately, there are techniques you can use to avoid poor performance.

<a name="cachingstrategy" />

## Caching Strategy

ListViews are often used to display much more data than can fit onscreen. Consider a music app, for example. A library of songs may have thousands of entries. The simple approach, which would be to create a row for every song, would have poor performance. That approach wastes valuable memory and can slow scrolling to a crawl. Another approach is to create and destroy rows as data is scrolled into view. This requires constant instantiation and cleanup of view objects, which can be very slow.

To conserve memory, the native [`ListView`](xref:Xamarin.Forms.ListView) equivalents for each platform have built-in features for re-using rows. Only the cells visible on screen are loaded in memory and the **content** is loaded into existing cells. This prevents the application from needing to instantiate thousands of objects, saving time and memory.

Xamarin.Forms permits [`ListView`](xref:Xamarin.Forms.ListView) cell re-use through the [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) enumeration, which has the following values:

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

The [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) caching strategy specifies that the [`ListView`](xref:Xamarin.Forms.ListView) will generate a cell for each item in the list, and is the default `ListView` behavior. It should generally be used in the following circumstances:

- When each cell has a large number of bindings (20-30+).
- When the cell template changes frequently.
- When testing reveals that the `RecycleElement` caching strategy results in a reduced execution speed.

It's important to recognize the consequences of the [`RetainElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RetainElement) caching strategy when working with custom cells. Any cell initialization code will need to run for each cell creation, which may be multiple times per second. In this circumstance, layout techniques that were fine on a page, like using multiple nested [`StackLayout`](xref:Xamarin.Forms.StackLayout) instances, become performance bottlenecks when they are setup and destroyed in real time as the user scrolls.

<a name="recycleelement" />

### RecycleElement

The [`RecycleElement`](xref:Xamarin.Forms.ListViewCachingStrategy.RecycleElement) caching strategy specifies that the [`ListView`](xref:Xamarin.Forms.ListView) will attempt to minimize its memory footprint and execution speed by recycling list cells. This mode does not always offer a performance improvement, and testing should be performed to determine any improvements. However, it is generally the preferred choice, and should be used in the following circumstances:

- When each cell has a small to moderate number of bindings.
- When each cell's [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) defines all of the cell data.
- When each cell is largely similar, with the cell template unchanging.

During virtualization the cell will have its binding context updated, and so if an application uses this mode it must ensure that binding context updates are handled appropriately. All data about the cell must come from the binding context or consistency errors may occur. This can be accomplished by using data binding to display cell data. Alternatively, cell data should be set in the `OnBindingContextChanged` override, rather than in the custom cell's constructor, as demonstrated in the following code example:

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

### Setting the Caching Strategy

The [`ListViewCachingStrategy`](xref:Xamarin.Forms.ListViewCachingStrategy) enumeration value is specified with a [`ListView`](xref:Xamarin.Forms.ListView) constructor overload, as shown in the following code example:

```csharp
var listView = new ListView(ListViewCachingStrategy.RecycleElement);
```

In XAML, set the `CachingStrategy` attribute as shown in the code below:

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

This has the same effect as setting the caching strategy argument in the constructor in C#; note that there is no `CachingStrategy` property on `ListView`.

#### Setting the Caching Strategy in a Subclassed ListView

Setting the `CachingStrategy` attribute from XAML on a subclassed [`ListView`](xref:Xamarin.Forms.ListView) will not produce the desired behavior, because there is no `CachingStrategy` property on `ListView`. In addition, if [XAMLC](~/xamarin-forms/xaml/xamlc.md) is enabled, the following error message will be produced: **No property, bindable property, or event found for 'CachingStrategy'**

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

<a name="improving-performance" />

## Improving ListView Performance

There are many techniques for improving the performance of a `ListView`:

-  Bind the `ItemsSource` property to an `IList<T>` collection instead of an `IEnumerable<T>` collection, because `IEnumerable<T>` collections don't support random access.
-  Use the built-in cells (like  `TextCell` / `SwitchCell` ) instead of  `ViewCell` whenever you can.
-  Use fewer elements. For example consider using a single `FormattedString` label instead of multiple labels.
-  Replace the `ListView` with a `TableView` when displaying non-homogenous data – that is, data of different types.
-  Limit the use of the [`Cell.ForceUpdateSize`](xref:Xamarin.Forms.Cell.ForceUpdateSize) method. If overused, it will degrade performance.
-  On Android, avoid setting a `ListView`'s row separator visibility or color after it has been instantiated, as it results in a large performance penalty.
-  Avoid changing the cell layout based on the [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext). This incurs large layout and initialization costs.
-  Avoid deeply nested layout hierarchies. Use  `AbsoluteLayout` or  `Grid` to help reduce nesting.
-  Avoid specific `LayoutOptions` other than  `Fill` (Fill is the cheapest to compute).
-  Avoid placing a `ListView` inside a `ScrollView` for the following reasons:
    - The `ListView` implements its own scrolling.
    - The `ListView` will not receive any gestures, as they will be handled by the parent `ScrollView`.
    - The `ListView` can present a customized header and footer that scrolls with the elements of the list, potentially offering the functionality that the `ScrollView` was used for. For more information see [Headers and Footers](~/xamarin-forms/user-interface/listview/customizing-list-appearance.md#Headers_and_Footers).
-  Consider a custom renderer if you need a very specific, complex design presented in your cells.

`AbsoluteLayout` has the potential to perform layouts without a single measure call. This makes it very powerful for performance. If `AbsoluteLayout` cannot be used, consider [`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout). If using `RelativeLayout`, passing Constraints directly will be considerably faster than using the expression API. That is because the expression API uses JIT, and on iOS the tree has to be interpreted, which is slower. The expression API is suitable for page layouts where it only required on initial layout and rotation, but in `ListView`, where it's run constantly during scrolling, it hurts performance.

Building a custom renderer for a [`ListView`](xref:Xamarin.Forms.ListView) or its cells is one approach to reducing the effect of layout calculations on scrolling performance. For more information, see [Customizing a ListView](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md) and [Customizing a ViewCell](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md).


## Related Links

- [Custom Renderer View (sample)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Custom Renderer ViewCell (sample)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/viewcell/)
- [ListViewCachingStrategy](xref:Xamarin.Forms.ListViewCachingStrategy)
