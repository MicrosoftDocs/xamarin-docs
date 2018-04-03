---
title: "Summary of Chapter 17. Mastering the Grid"
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 71EDEF9C-4220-4D2E-A235-43F1EC8746C1
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
---

# Summary of Chapter 17. Mastering the Grid

The [`Grid`](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/) is a powerful layout mechanism that arranges its children into rows and columns of cells. Unlike the similar HTML `table` element, the `Grid` is solely for purposes of layout rather than presentation.

## The basic Grid

`Grid` derives from [`Layout<View>`](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout%3CT%3E/), which defines a [`Children`](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/) property that `Grid` inherits. You can fill this collection in either XAML or code.

### The Grid in XAML

The definition of a `Grid` in XAML generally begins with filling the [`RowDefinitions`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowDefinitions/) and [`ColumnDefinitions`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnDefinitions/) collections of the `Grid` with [`RowDefinition`](https://developer.xamarin.com/api/type/Xamarin.Forms.RowDefinition/) and [`ColumnDefinition`](https://developer.xamarin.com/api/type/Xamarin.Forms.ColumnDefinition/) objects. This is how you establish the number of rows and columns of the `Grid`, and their properties.

`RowDefinition` has a [`Height`](https://developer.xamarin.com/api/property/Xamarin.Forms.RowDefinition.Height/) property and `ColumnDefinition` has a [`Width`](https://developer.xamarin.com/api/property/Xamarin.Forms.ColumnDefinition.Width/) property, both of type [`GridLength`](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLength/), a structure.

In XAML, the [`GridLengthTypeConverter`](https://developer.xamarin.com/api/type/Xamarin.Forms.GridLengthTypeConverter/) converts simple text strings into `GridLength` values. Behind the scenes, the [`GridLength` constructor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.GridLength.GridLength/p/System.Double/Xamarin.Forms.GridUnitType/) creates the `GridLength` value based on a number and a value of type [`GridUnitType`](https://developer.xamarin.com/api/type/Xamarin.Forms.GridUnitType/), an enumeration with three members:

- [`Absolute`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Absolute/) &mdash; the width or height is specified in device-independent units (a number in XAML)
- [`Auto`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Auto/) &mdash; the height or width is autosized based on cell contents ("Auto" in XAML)
- [`Star`](https://developer.xamarin.com/api/field/Xamarin.Forms.GridUnitType.Star/) &mdash; leftover height or width is allocated proportionally (a number with "\*", called *star*, in XAML)

Each child of the `Grid` must also be assigned a row and column (either explicitly or implicitly). Row spans and column spans are optional. These are all specified using attached bindable properties &mdash; properties that are defined by the `Grid` but set on children of the `Grid`. `Grid` defines four static attached bindable properties:

- [`RowProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowProperty/) &mdash; the zero-based row; default is 0
- [`ColumnProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnProperty/) &mdash; the zero-based column; default is 0
- [`RowSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.RowSpanProperty/) &mdash; the number of rows the child spans; default is 1
- [`ColumnSpanProperty`](https://developer.xamarin.com/api/field/Xamarin.Forms.Grid.ColumnSpanProperty/) &mdash; the number of columns the child spans; default is 1

In code, a program can use eight static methods to set and get these values:

- [`Grid.SetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRow/p/Xamarin.Forms.BindableObject/System.Int32/) and [`Grid.GetRow`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRow/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumn/p/Xamarin.Forms.BindableObject/System.Int32/) and [`Grid.GetColumn`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumn/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetRowSpan/p/Xamarin.Forms.BindableObject/System.Int32/) and [`Grid.GetRowSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetRowSpan/p/Xamarin.Forms.BindableObject/)
- [`Grid.SetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.SetColumnSpan/p/Xamarin.Forms.BindableObject/System.Int32/) and [`Grid.GetColumnSpan`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid.GetColumnSpan/p/Xamarin.Forms.BindableObject/)

In XAML you use the following attributes for setting these values:

- `Grid.Row`
- `Grid.Column`
- `Grid.RowSpan`
- `Grid.ColumnSpan`

The [**SimpleGridDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SimpleGridDemo) sample demonstrates creating and initializing a `Grid` in XAML.

The `Grid` inherits the [`Padding`](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout.Padding/) property from `Layout` and defines two additional properties that provide spacing between the rows and columns:

- [`RowSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.RowSpacing/) has a default value of 6
- [`ColumnSpacing`](https://developer.xamarin.com/api/property/Xamarin.Forms.Grid.ColumnSpacing/) has a default value of 6

The `RowDefinitions` and `ColumnDefinitions` collections aren't strictly required. If absent, the `Grid` creates rows and columns for the `Grid` children and gives them all a default `GridLength` of "\*" (star).

### The Grid in code

The [**GridCodeDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCodeDemo) sample demonstrates how to create and populate a `Grid` in code. You can set the attached properties for each child directly, or indirectly by calling additional `Add` methods such as [`Add`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.Add/p/Xamarin.Forms.View/System.Int32/System.Int32/System.Int32/System.Int32/) defined by the [Grid.IGridList<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid+IGridList%3CT%3E/) interface.

### The Grid bar chart

The [**GridBarChart**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridBarChart) sample shows how to add multiple `BoxView` elements to a `Grid` using the bulk [`AddHorizontal`](https://developer.xamarin.com/api/member/Xamarin.Forms.Grid+IGridList%3CT%3E.AddHorizontal/p/System.Collections.Generic.IEnumerable%7BXamarin.Forms.View%7D/) method. By default, these `BoxView` elements have equal width. The height of each `BoxView` can then be controlled to resemble a bar chart.

The `Grid` in the **GridBarChart** sample shares an `AbsoluteLayout` parent with an initially invisible `Frame`. The program also sets a `TapGestureRecognizer` on each `BoxView` to use the `Frame` to display information about the tapped bar.

### Alignment in the Grid

The [**GridAlignment**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridAlignment) sample demonstrates how to use the `VerticalOptions` and `HorizontalOptions` properties to align children in a `Grid` cell.

The [**SpacingButtons**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/SpacingButtons) sample equally spaces `Button` elements centered in `Grid` cells.

### Cell dividers and borders

The `Grid` does not include a feature that draws cell dividers or borders. However, you can make your own.

The [**GridCellDividers**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellDividers) demonstrates how to define additional rows and column specifically for thin `BoxView` elements to mimic dividing lines.

The [**GridCellBorders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridCellBorders) program does not create any additional cells but instead aligns `BoxView` elements in each cell to mimic a cell border.

## Almost real-life Grid examples

The [**KeypadGrid**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/KeypadGrid) sample uses a `Grid` to display a keypad:

[![Triple screenshot of Keypad Grid](images/ch17fg12-small.png "Keypad Grid")](images/ch17fg12-large.png#lightbox "Keypad Grid")

### Responding to orientation changes

The `Grid` can help structure a program to respond to orientation changes. The
[**GridRgbSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17/GridRgbSliders) sample demonstrates a technique that moves an element between a second row of a portrait-oriented phone and the second column of a landscape-oriented phone.

The program initializes `Slider` elements to a range of 0 to 255, and uses data bindings to display the value of the sliders in hexadecimal. Because the `Slider` values are floating point, and the .NET formatting string for hexadecimal only works with integers, a [`DoubleToIntConvert`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DoubleToIntConverter.cs) class in the [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) library helps out.



## Related Links

- [Chapter 17 full text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch17-Apr2016.pdf)
- [Chapter 17 samples](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter17)
- [Grid](~/xamarin-forms/user-interface/layouts/grid.md)
