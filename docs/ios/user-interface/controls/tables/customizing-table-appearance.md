---
title: "Customizing a Table's Appearance in Xamarin.iOS"
description: "This document describes how to customize a table's appearance in Xamarin.iOS. It discusses cell styles, accessories, cell separators, and custom cell layouts."
ms.service: xamarin
ms.assetid: 8A83DE38-0028-CB61-66F9-0FB9DE552286
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
no-loc: [Objective-C]
---

# Customizing a Table's Appearance in Xamarin.iOS

The simplest way to change the appearance of a table is to use a different
cell style. You can change which cell style is used when creating each cell in
the `UITableViewSource`’s `GetCell` method.

## Cell Styles

There are four built-in styles:

- **Default** – supports a `UIImageView`.
- **Subtitle** – supports a `UIImageView` and subtitle.
- **Value1** – right aligned subtitle, supports a `UIImageView`.
- **Value2** – title is right-aligned and subtitle is left-aligned (but no image).

These screenshots show how each style appears:

 [![These screenshots show how each style appears](customizing-table-appearance-images/image7.png)](customizing-table-appearance-images/image7.png#lightbox)

The sample **CellDefaultTable** contains the code to produce these screens. The
cell style is set in the `UITableViewCell` constructor, like
this:

```csharp
cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Subtitle, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value1, cellIdentifier);
//cell = new UITableViewCell (UITableViewCellStyle.Value2, cellIdentifier);
```

[Supported properties](xref:UIKit.UITableViewCell) of the cell style can then be set:

```csharp
cell.TextLabel.Text = tableItems[indexPath.Row].Heading;
cell.DetailTextLabel.Text = tableItems[indexPath.Row].SubHeading;
cell.ImageView.Image = UIImage.FromFile("Images/" + tableItems[indexPath.Row].ImageName); // don't use for Value2
```

## Accessories

Cells can have the following accessories added to the right of the view:

- **Checkmark** – can be used to indicate multiple-selection in a table.
- **DetailButton** – responds to touch independently of the rest of the cell, allowing it to perform a different function to touching the cell itself (such as opening a popup or new window that is not part of a `UINavigationController` stack).
- **DisclosureIndicator** – normally used to indicate that touching the cell will open another view.
- **DetailDisclosureButton** – a combination of the `DetailButton` and `DisclosureIndicator`.

This is what they look like:

 [![Sample Accessories](customizing-table-appearance-images/image8.png)](customizing-table-appearance-images/image8.png#lightbox)

To display one of these accessories you can set the `Accessory`
property in the `GetCell` method:

```csharp
cell.Accessory = UITableViewCellAccessory.Checkmark;
//cell.Accessory = UITableViewCellAccessory.DisclosureIndicator;
//cell.Accessory = UITableViewCellAccessory.DetailDisclosureButton; // implement AccessoryButtonTapped
//cell.Accessory = UITableViewCellAccessory.None; // to clear the accessory
```

When the `DetailButton` or `DetailDisclosureButton` are shown, you should also
override the `AccessoryButtonTapped` to perform some action when it
is touched.

```csharp
public override void AccessoryButtonTapped (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("DetailDisclosureButton Touched", tableItems[indexPath.Row].Heading, UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    owner.PresentViewController (okAlertController, true, null);

    tableView.DeselectRow (indexPath, true);
}
```

The sample **CellAccessoryTable** shows an example using accessories.

## Cell Separators

Cell separators are table cells used to separate the table. The properties are set on the Table.

```csharp
TableView.SeparatorColor = UIColor.Blue;
TableView.SeparatorStyle = UITableViewCellSeparatorStyle.DoubleLineEtched;
```

It is also possible to add a blur or vibrancy effect to the separator:

```csharp
// blur effect
TableView.SeparatorEffect =
    UIBlurEffect.FromStyle(UIBlurEffectStyle.Dark);

//vibrancy effect
var effect = UIBlurEffect.FromStyle(UIBlurEffectStyle.Light);
TableView.SeparatorEffect = UIVibrancyEffect.FromBlurEffect(effect);
```

The separator can also have an inset:

```csharp
TableView.SeparatorInset.InsetRect(new CGRect(4, 4, 150, 2));
```

## Creating Custom Cell Layouts

To change the visual style of a table you need to supply custom cells for it
to display. The custom cell can have different colors and control layouts.

The CellCustomTable example implements a `UITableViewCell`
subclass that defines a custom layout of `UILabel`s and a `UIImage` with different fonts and colors. The resulting cells look like this:

 [![Custom Cell Layouts](customizing-table-appearance-images/image9.png)](customizing-table-appearance-images/image9.png#lightbox)

The custom cell class consists of only three methods:

- **Constructor** – creates the UI controls and sets the custom style properties (eg. font face, size and colors).
- **UpdateCell** – a method for  `UITableView.GetCell` to use to set the cell’s properties.
- **LayoutSubviews** – set the location of the UI controls. In the example every cell has the same layout, but a more complex cell (particularly those with varying sizes) might need different layout positions depending on the content being displayed.

The complete sample code in **CellCustomTable > CustomVegeCell.cs** follows:

```csharp
public class CustomVegeCell : UITableViewCell  {
    UILabel headingLabel, subheadingLabel;
    UIImageView imageView;
    public CustomVegeCell (NSString cellId) : base (UITableViewCellStyle.Default, cellId)
    {
        SelectionStyle = UITableViewCellSelectionStyle.Gray;
        ContentView.BackgroundColor = UIColor.FromRGB (218, 255, 127);
        imageView = new UIImageView();
        headingLabel = new UILabel () {
            Font = UIFont.FromName("Cochin-BoldItalic", 22f),
            TextColor = UIColor.FromRGB (127, 51, 0),
            BackgroundColor = UIColor.Clear
        };
        subheadingLabel = new UILabel () {
            Font = UIFont.FromName("AmericanTypewriter", 12f),
            TextColor = UIColor.FromRGB (38, 127, 0),
            TextAlignment = UITextAlignment.Center,
            BackgroundColor = UIColor.Clear
        };
        ContentView.AddSubviews(new UIView[] {headingLabel, subheadingLabel, imageView});

    }
    public void UpdateCell (string caption, string subtitle, UIImage image)
    {
        imageView.Image = image;
        headingLabel.Text = caption;
        subheadingLabel.Text = subtitle;
    }
    public override void LayoutSubviews ()
    {
        base.LayoutSubviews ();
        imageView.Frame = new CGRect (ContentView.Bounds.Width - 63, 5, 33, 33);
        headingLabel.Frame = new CGRect (5, 4, ContentView.Bounds.Width - 63, 25);
        subheadingLabel.Frame = new CGRect (100, 18, 100, 20);
    }
}
```

The `GetCell` method of the `UITableViewSource` needs
to be modified to create the custom cell:

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (cellIdentifier) as CustomVegeCell;
    if (cell == null)
        cell = new CustomVegeCell (cellIdentifier);
    cell.UpdateCell (tableItems[indexPath.Row].Heading
            , tableItems[indexPath.Row].SubHeading
            , UIImage.FromFile ("Images/" + tableItems[indexPath.Row].ImageName) );
    return cell;
}
```

## Related Links

- [WorkingWithTables (sample)](/samples/xamarin/ios-samples/workingwithtables)