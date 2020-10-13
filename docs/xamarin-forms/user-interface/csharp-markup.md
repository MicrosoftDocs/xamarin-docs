---
title: "Xamarin.Forms C# markup"
description: "C# markup is an opt-in set of fluent helper methods and classes to simplify the process of building declarative Xamarin.Forms user interfaces in C#."
ms.prod: xamarin
ms.assetid: D41B9DCD-5C34-4C2F-B177-FC082AB2E9E0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/15/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms C# markup

![Pre-release API](~/media/shared/preview.png)

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)

C# markup is an opt-in set of fluent helper methods and classes to simplify the process of building declarative Xamarin.Forms user interfaces in C#. The fluent API provided by C# markup is available in the `Xamarin.Forms.Markup` namespace.

Just as with XAML, C# markup enables a clean separation between UI markup and UI logic. This can be achieved by separating UI markup and UI logic into distinct partial class files. For example, for a login page the UI markup would be in a file named *LoginPage.cs*, while the UI logic would be in a file named *LoginPage.logic.cs*.

C# markup is available from Xamarin.Forms 4.6. However, it's currently experimental and can only be used by adding the following line of code to your *App.cs* file:

```csharp
Device.SetFlags(new string[]{ "Markup_Experimental" });
```

> [!NOTE]
> C# markup is available on all the platforms supported by Xamarin.Forms.

## Basic example

The following example shows setting the page content to a new [`Grid`](xref:Xamarin.Forms.Grid) containing a [`Label`](xref:Xamarin.Forms.Label) and an [`Entry`](xref:Xamarin.Forms.Entry), in C#:

```csharp
Grid grid = new Grid();

Label label = new Label { Text = "Code: " };
grid.Children.Add(label, 0, 1);

Entry entry = new Entry
{
    Placeholder = "Enter number",
    Keyboard = Keyboard.Numeric,
    BackgroundColor = Color.AliceBlue,
    TextColor = Color.Black,
    FontSize = 15,
    HeightRequest = 44,
    Margin = fieldMargin
};
grid.Children.Add(entry, 0, 2);
Grid.SetColumnSpan(entry, 2);
entry.SetBinding(Entry.TextProperty, new Binding("RegistrationCode"));

Content = grid;
```

This example creates a [`Grid`](xref:Xamarin.Forms.Grid) object, with child [`Label`](xref:Xamarin.Forms.Label) and [`Entry`](xref:Xamarin.Forms.Entry) objects. The `Label` displays text, and the `Entry` data binds to the `RegistrationCode` property of the viewmodel. Each child view is set to appear in a specific row in the `Grid`, and the `Entry` spans all the columns in the `Grid`. In addition, the height of the `Entry` is set, along with its keyboard, colors, the font size of its text, and its `Margin`. Finally, the `Page.Content` property is set to the `Grid` object.

C# markup enables this code to be re-written using its fluent API:

```csharp
using Xamarin.Forms.Markup;
using static Xamarin.Forms.Markup.GridRowsColumns;

Content = new Grid
{
  Children =
  {
    new Label { Text = "Code:" }
               .Row (BodyRow.CodeHeader) .Column (BodyCol.Header),

    new Entry { Placeholder = "Enter number", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
               .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
               .Bind (nameof(vm.RegistrationCode))
  }
}};
```

This example is identical to the previous example, but the C# markup fluent API simplifies the process of building the UI in C#.

> [!NOTE]
> C# markup includes extension methods that set specific view properties. These extension methods are not meant to replace all property setters. Instead, they are designed to improve code readability, and can be used in combination with property setters. It's recommended to always use an extension method when one exists for a property, but you can choose your preferred balance.

## Data binding

C# markup includes a `Bind` extension method, along with overloads, that creates a data binding between a view bindable property and a specified property. The `Bind` method knows the default bindable property for the majority of the controls that are included in Xamarin.Forms. Therefore, it's typically not necessary to specify the target property when using this method. However, you can also register the default bindable property for additional controls:

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.Register(HoverButton.CommandProperty, RadialGauge.ValueProperty);
```

The `Bind` method can be used to bind to any bindable property:

```csharp
using Xamarin.Forms.Markup;
// ...

new Label { Text = "No data available" }
           .Bind (Label.IsVisibleProperty, nameof(vm.Empty))
```

In addition, the `BindCommand` extension method can bind to a control's default `Command` and `CommandParameter` properties in a single method call:

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap me" }
              .BindCommand (nameof(vm.TapCommand))
```

By default, the `CommandParameter` is bound to the binding context. You can also specify the binding path and source for the `Command` and the `CommandParameter` bindings:

```csharp
using Xamarin.Forms.Markup;
// ...

new TextCell { Text = "Tap Me" }
              .BindCommand (nameof(vm.TapCommand), vm, nameof(Item.Id))
```

In this example, the binding context is an `Item` instance, so you don't need to specify a source for the `Id` `CommandParameter` binding.

If you only need to bind to `Command`, you can pass `null` to the `parameterPath` argument of the `BindCommand` method. Alternatively, use the `Bind` method.

You can also register the default `Command` and `CommandParameter` properties for additional controls:

```csharp
using Xamarin.Forms.Markup;
//...

DefaultBindableProperties.RegisterCommand(
    (CustomViewA.CommandProperty, CustomViewA.CommandParameterProperty),
    (CustomViewB.CommandProperty, CustomViewB.CommandParameterProperty)
);
```

Inline converter code can be passed into the `Bind` method with the `convert` and `convertBack` parameters:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth),
                  convert: (int depth) => new Thickness(depth * 20, 0, 0, 0))
```

Type safe converter parameters are also supported:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { }
           .Bind (nameof(viewModel.Text),
                  convert: (string text, int repeat) => string.Concat(Enumerable.Repeat(text, repeat)))
```

In addition, converter code and instances can be re-used with the `FuncConverter` class:

```csharp
using Xamarin.Forms.Markup;
//...

FuncConverter<int, Thickness> treeMarginConverter = new FuncConverter<int, Thickness>(depth => new Thickness(depth * 20, 0, 0, 0));
new Label { Text = "Tree" }
           .Bind (Label.MarginProperty, nameof(TreeNode.TreeDepth), converter: treeMarginConverter),
```

The `FuncConverter` class also supports `CultureInfo` objects:

```csharp
using Xamarin.Forms.Markup;
//...

cultureAwareConverter = new FuncConverter<DateTimeOffset, string, int>(
    (date, daysToAdd, culture) => date.AddDays(daysToAdd).ToString(culture)
);
```

It's also possible to data bind to `Span` objects that are specified with the `FormattedText` property:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { } .FormattedText (
    new Span { Text = "Built with " },
    new Span { TextColor = Color.Blue, TextDecorations = TextDecorations.Underline }
              .BindTapGesture (nameof(vm.ContinueToCSharpForMarkupCommand))
              .Bind (nameof(vm.Title))
)
```

### Gesture recognizers

`Command` and `CommandParameter` properties can be data bound to `GestureElement` and `View` types using the `BindClickGesture`, `BindSwipeGesture`, and `BindTapGesture` extension methods:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .BindTapGesture (nameof(vm.TapCommand))
```

This example creates a gesture recognizer of the specified type, and adds it to the [`Label`](xref:Xamarin.Forms.Label). The `Bind*Gesture` extension methods offer the same parameters as the `BindCommand` extension methods. However, by default `Bind*Gesture` does not bind `CommandParameter`, while `BindCommand` does.

To initialize a gesture recognizer with parameters, use the `ClickGesture`, `PanGesture`, `PinchGesture`, `SwipeGesture`, and `TapGesture` extension methods:

```csharp
using Xamarin.Forms.Markup;
//...

new Label { Text = "Tap Me" }
           .TapGesture (g => g.Bind(nameof(vm.DoubleTapCommand)).NumberOfTapsRequired = 2)
```

Since a gesture recognizer is a `BindableObject`, you can use the `Bind` and `BindCommand` extension methods when you initialize it. You can also initialize custom gesture recognizer types with the `Gesture<TGestureElement, TGestureRecognizer>` extension method.

## Layout

C# markup includes a series of layout extension methods that support positioning views in layouts, and content in views:

| Type | Extension methods |
|---|---|
| `FlexLayout` | `AlignSelf`, `Basis`, `Grow`, `Menu`, `Order`, `Shrink` |
| `Grid` | `Row`, `Column`, `RowSpan`, `ColumnSpan` |
| `Label` | `TextLeft`, `TextCenterHorizontal`, `TextRight` <br/> `TextTop`, `TextCenterVertical`, `TextBottom` <br/> `TextCenter` |
| `Layout` | `Padding`, `Paddings` |
| `LayoutOptions` | `Left`, `CenterHorizontal`, `FillHorizontal`, `Right` <br/> `LeftExpand`, `CenterExpandHorizontal`, `FillExpandHorizontal`, `RightExpand` <br /> `Top`, `Bottom`, `CenterVertical`, `FillVertical` <br /> `TopExpand`, `BottomExpand`, `CenterExpandVertical`, `FillExpandVertical` <br /> `Center`, `Fill`, `CenterExpand`, `FillExpand` |
| `View` | `Margin`, `Margins` |
| `VisualElement` | `Height`, `Width`, `MinHeight`, `MinWidth`, `Size`, `MinSize` |

### Left-to-right and right-to-left support

For C# markup that is designed to support either left-to-right (LTR) or right-to-left (RTL) flow direction, the extension methods listed above offer the most intuitive set of names: `Left`, `Right`, `Top` and `Bottom`.

To make the correct set of left and right extension methods available, and in the process make explicit which flow direction the markup is designed for, include one of the following two `using` directives: `using Xamarin.Forms.Markup.LeftToRight;`, or `using Xamarin.Forms.Markup.RightToLeft;`.

For C# markup that is designed to support both left-to-right and right-to-left flow direction, it's recommended to use the extension methods in the following table rather than either of the above namespaces:

| Type | Extension methods |
|---|---|
| `Label` | `TextStart`, `TextEnd` |
| `LayoutOptions` | `Start`, `End` <br/> `StartExpand`, `EndExpand` |

### Layout line convention

The recommended convention is to put all the layout extension methods for a view on a single line in the following order:

1. The row and column that contain the view.
1. Alignment within the row and column.
1. Margins around the view.
1. View size.
1. Padding within the view.
1. Content alignment within the padding.

The following code shows an example of this convention:

```csharp
new Label { }
           .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal () // Layout line
```

Consistently following the convention enables you to quickly read C# markup and build a mental map of where the view content is located in the UI.

## Grid rows and columns

Enumerations can be used to define [`Grid`](xref:Xamarin.Forms.Grid) rows and columns, instead of using numbers. This offers the advantage that renumbering is not required when adding or removing rows or columns.

> [!IMPORTANT]
> Defining [`Grid`](xref:Xamarin.Forms.Grid) rows and columns using enumerations requires the following `using` directive: `using static Xamarin.Forms.Markup.GridRowsColumns;`

The following code shows an example of how to define and consume [`Grid`](xref:Xamarin.Forms.Grid) rows and columns using enumerations:

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms.Markup.LeftToRight;
using static Xamarin.Forms.Markup.GridRowsColumns;
// ...

enum BodyRow
{
    Prompt,
    CodeHeader,
    CodeEntry,
    Button
}

enum BodyCol
{
    FieldLabel,
    FieldValidation
}

View Build() => new Grid
{
    RowDefinitions = Rows.Define(
        (BodyRow.Prompt    , 170 ),
        (BodyRow.CodeHeader, 75  ),
        (BodyRow.CodeEntry , Auto),
        (BodyRow.Button    , Auto)
    ),

    ColumnDefinitions = Columns.Define(
        (BodyCol.FieldLabel     , 160 ),
        (BodyCol.FieldValidation, Star)
    ),

    Children =
    {
        new Label { LineBreakMode = LineBreakMode.WordWrap } .Font (15) .Bold ()
                   .Row (BodyRow.Prompt) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .CenterVertical () .Margin (fieldNameMargin) .TextCenterHorizontal ()
                   .Bind (nameof(vm.RegistrationPrompt)),

        new Label { Text = "Registration code" } .Bold ()
                   .Row (BodyRow.CodeHeader) .Column(BodyCol.FieldLabel) .Bottom () .Margin (fieldNameMargin),

        new Label { } .Italic ()
                   .Row (BodyRow.CodeHeader) .Column (BodyCol.FieldValidation) .Right () .Bottom () .Margin (fieldNameMargin)
                   .Bind (nameof(vm.RegistrationCodeValidationMessage)),

        new Entry { Placeholder = "E.g. 123456", Keyboard = Keyboard.Numeric, BackgroundColor = Color.AliceBlue, TextColor = Color.Black } .Font (15)
                   .Row (BodyRow.CodeEntry) .ColumnSpan (All<BodyCol>()) .Margin (fieldMargin) .Height (44)
                   .Bind (nameof(vm.RegistrationCode), BindingMode.TwoWay),

        new Button { Text = "Verify" } .Style (FilledButton)
                    .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (PageMarginSize)
                    .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode))
                    .Bind (nameof(vm.VerifyRegistrationCodeCommand)),
    }
};
```

In addition, you can concisely define rows and columns without enumerations:

```csharp
new Grid
{
    RowDefinitions = Rows.Define (Auto, Star, 20),
    ColumnDefinitions = Columns.Define (Auto, Star, 20, 40)
    // ...
}
```

## Fonts

The controls in the following list can call the `FontSize`, `Bold`, `Italic`, and `Font` extension methods to set the appearance of the text displayed by the control:

- `Button`
- `DatePicker`
- `Editor`
- `Entry`
- `Label`
- `Picker`
- `SearchBar`
- `Span`
- `TimePicker`

## Effects

Effects can be attached to controls with the `Effect` extension method:

```csharp
using Xamarin.Forms.Markup;
// ...

new Button { Text = "Tap Me" }
            .Effects (new ButtonMixedCaps())
```

## Logic integration

The `Invoke` extension method can be used to execute code inline in your C# markup:

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Invoke (l => l.ItemTapped += OnListViewItemTapped)
```

In addition, you can use the `Assign` extension method to access a control from outside the UI markup (in the UI logic file):

```csharp
using Xamarin.Forms.Markup;
// ...

new ListView { } .Assign (out MyListView)
```

## Styles

The following example shows how to create implicit and explicit styles using C# markup:

```csharp
using Xamarin.Forms.Markup;
using Xamarin.Forms;

namespace CSharpForMarkupDemos
{
    public static class Styles
    {
        static Style<Button> buttons, filledButton;
        static Style<Label> labels;
        static Style<Span> link;

        #region Implicit styles

        public static ResourceDictionary Implicit => new ResourceDictionary { Buttons, Labels };

        public static Style<Button> Buttons => buttons ?? (buttons = new Style<Button>(
            (Button.HeightRequestProperty, 44),
            (Button.FontSizeProperty, 13),
            (Button.HorizontalOptionsProperty, LayoutOptions.Center),
            (Button.VerticalOptionsProperty, LayoutOptions.Center)
        ));

        public static Style<Label> Labels => labels ?? (labels = new Style<Label>(
            (Label.FontSizeProperty, 13),
            (Label.TextColorProperty, Color.Black)
        ));

        #endregion Implicit styles

        #region Explicit styles

        public static Style<Button> FilledButton => filledButton ?? (filledButton = new Style<Button>(
            (Button.TextColorProperty, Color.White),
            (Button.BackgroundColorProperty, Color.FromHex("#1976D2")),
            (Button.CornerRadiusProperty, 5)
        )).BasedOn(Buttons);

        public static Style<Span> Link => link ?? (link = new Style<Span>(
            (Span.TextColorProperty, Color.Blue),
            (Span.TextDecorationsProperty, TextDecorations.Underline)
        ));

        #endregion Explicit styles
    }
}
```

The implicit styles can be consumed by loading them into the application resource dictionary:

```csharp
public App()
{
    Resources = Styles.Implicit;
    // ...
}
```

Explicit styles can be consumed with the `Style` extension method.

```csharp
using static CSharpForMarkupExample.Styles;
// ...

new Button { Text = "Tap Me" } .Style (FilledButton),
```

> [!NOTE]
> In addition to the `Style` extension method, there are also `ApplyToDerivedTypes`, `BasedOn`, `Add`, and `CanCascade` extension methods.

Alternatively, you can create your own styling extension methods:

```csharp
public static TButton Filled<TButton>(this TButton button) where TButton : Button
{
    button.Buttons(); // Equivalent to Style .BasedOn (Buttons)
    button.TextColor = Color.White;
    button.BackgroundColor = Color.Red;
    return button;
}
```

The `Filled` extension method can then be consumed as follows:

```csharp
new Button { Text = "Tap Me" } .Filled ()
```

## Platform-specifics

The `Invoke` extension method can be used to apply platform-specifics. However, to avoid ambiguity errors, don't include `using` directives for the `Xamarin.Forms.PlatformConfiguration.*Specific` namespaces directly. Instead, create a namespace alias and consume the platform-specific via the alias:

```csharp
using Xamarin.Forms.Markup;
using PciOS = Xamarin.Forms.PlatformConfiguration.iOSSpecific;
// ...

new ListView { } .Invoke (l => PciOS.ListView.SetGroupHeaderStyle(l, PciOS.GroupHeaderStyle.Grouped))
```

In addition, if you consume certain platform-specifics frequently you can create fluent extension methods for them in your own extensions class:

```csharp
public static T iOSGroupHeaderStyle<T>(this T listView, PciOS.GroupHeaderStyle style) where T : Forms.ListView
{
  PciOS.ListView.SetGroupHeaderStyle(listView, style);
  return listView;
}
```

The extension method can then be consumed as follows:

```csharp
new ListView { } .iOSGroupHeaderStyle(PciOS.GroupHeaderStyle.Grouped)
```

For more information about platform-specifics, see [Android platform features](~/xamarin-forms/platform/android/index.md), [iOS platform features](~/xamarin-forms/platform/ios/index.md), and [Windows platform features](~/xamarin-forms/platform/windows/index.md).

## Recommended convention

A recommended order and grouping of properties and helper methods is:

- **Purpose**: any property or helper methods whose value identifies the control's purpose (e.g. `Text`, `Placeholder`, .`Assign`).
- **Other**: all properties or helper methods that are not layout or binding, on the same line or multiple lines.
- **Layout**: layout is ordered inwards: rows and columns, layout options, margin, size, padding, and content alignment.
- **Bind**: data binding is performed at the end of the method chain, with one bound property per line. If the *default* bindable property is bound, it should be at the end of the method chain.

The following code shows an example of following this convention:

```csharp
new Button { Text = "Verify" /* purpose */ } .Style (FilledButton) // other
            .Row (BodyRow.Button) .ColumnSpan (All<BodyCol>()) .FillExpandHorizontal () .Margin (10) // layout
            .Bind (Button.IsVisibleProperty, nameof(vm.CanVerifyRegistrationCode)) // bind
            .Bind (nameof(vm.VerifyRegistrationCodeCommand)), // bind default

new Label { }
           .Assign (out animatedMessageLabel) // purpose
           .Invoke (label => label.SizeChanged += MessageLabel_SizeChanged) // other
           .Row (BodyRow.Message) .ColumnSpan (All<BodyCol>()) // layout
           .Bind (nameof(vm.Message)), // bind default
```

Consistently applying this convention enables you to quickly scan your C# markup and build a mental image of the UI layout.

## Related links

- [CSharpForMarkupDemos (sample)](/samples/xamarin/xamarin-forms-samples/userinterface-csharpmarkupdemos/)
- [Android platform features](~/xamarin-forms/platform/android/index.md)
- [iOS platform features](~/xamarin-forms/platform/ios/index.md)
- [Windows platform features](~/xamarin-forms/platform/windows/index.md)