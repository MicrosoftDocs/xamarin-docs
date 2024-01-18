---
title: "Editing Tables with Xamarin.iOS"
description: "This document describes how to edit tables in Xamarin.iOS. It discusses swipe to delete, edit mode, and row insertion."
ms.service: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
no-loc: [Objective-C]
---

# Editing Tables with Xamarin.iOS

Table editing features are enabled by overriding methods in a `UITableViewSource` subclass. The simplest editing behavior is the
swipe-to-delete gesture that can be implemented with a single method override.
More complex editing (including moving rows) can be done with the table in edit
mode.

## Swipe to Delete

The swipe to delete feature is a natural gesture in iOS that users expect. 

 [![Example of Swipe to Delete](editing-images/image10.png)](editing-images/image10.png#lightbox)

There are three method overrides that affect the swipe gesture to show a **Delete** button in a cell:

- **CommitEditingStyle** – The table source detects if this method is overridden and automatically enables the swipe-to-delete gesture. The method’s implementation should call  `DeleteRows` on the  `UITableView` to cause the cells to disappear, and also remove the underlying data from your model (for example, an array, dictionary or database). 
- **CanEditRow** – If CommitEditingStyle is overridden, all rows are assumed to be editable. If this method is implemented and returns false (for some specific rows, or for all rows) then the swipe-to-delete gesture will not be available in that cell. 
- **TitleForDeleteConfirmation** – Optionally specifies the text for the  **Delete** button. If this method is not implemented the button text will be “Delete”. 

These methods are implemented in the `TableSource` class follows:

```csharp
public override void CommitEditingStyle (UITableView tableView, UITableViewCellEditingStyle editingStyle, Foundation.NSIndexPath indexPath)
{
    switch (editingStyle) {
        case UITableViewCellEditingStyle.Delete:
            // remove the item from the underlying data source
            tableItems.RemoveAt(indexPath.Row);
            // delete the row from the table
            tableView.DeleteRows (new NSIndexPath[] { indexPath }, UITableViewRowAnimation.Fade);
            break;
        case UITableViewCellEditingStyle.None:
            Console.WriteLine ("CommitEditingStyle:None called");
            break;
    }
}
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override string TitleForDeleteConfirmation (UITableView tableView, NSIndexPath indexPath)
{   // Optional - default text is 'Delete'
    return "Trash (" + tableItems[indexPath.Row].SubHeading + ")";
}
```

For this example the `UITableViewSource` has been updated to use a `List<TableItem>` (instead of a string array) as the data source because it supports adding and deleting items from the collection.

## Edit Mode

When a table is in edit mode the user sees a red ‘stop’ widget on each
row, which reveals a Delete button when touched. The table also displays a
‘handle’ icon to indicate that the row can be dragged to change the order.
The **TableEditMode** sample implements these features as shown.

 [![The TableEditMode sample implements these features as shown](editing-images/image11.png)](editing-images/image11.png#lightbox)

There are a number of different methods on `UITableViewSource`
that affect a table’s edit mode behavior:

- **CanEditRow** – whether each row can be edited. Return false to prevent both swipe-to-delete and deletion while in edit mode. 
- **CanMoveRow** – return true to enable the move ‘handle’ or false to prevent moving. 
- **EditingStyleForRow** – when the table is in edit mode, the return value from this method determines whether the cell displays the red deletion icon or the green add icon. Return  `UITableViewCellEditingStyle.None` if the row should not be editable. 
- **MoveRow** – called when a row is moved so that the underlying data structure can be modified to match the data as it is displayed in the table. 

The implementation for the first three methods is relatively straight forward – unless you
wish to use the `indexPath` to change the behavior of specific rows,
just hardcode the return values for the entire table.

```csharp
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you don't allow re-ordering
}
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    return UITableViewCellEditingStyle.Delete; // this example doesn't support Insert
}
```

The `MoveRow` implementation is a little more complicated because
it needs to alter the underlying data structure to match the new order. Because
the data is implemented as a `List` the code below deletes the data item at its
old location and inserts it at the new location. If the data was stored in a
SQLite database table with an ‘order’ column (for example), this method
would instead need to perform some SQL operations to reorder the numbers in that
column.

```csharp
public override void MoveRow (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    var item = tableItems[sourceIndexPath.Row];
    var deleteAt = sourceIndexPath.Row;
    var insertAt = destinationIndexPath.Row;
    
    // are we inserting 
    if (destinationIndexPath.Row < sourceIndexPath.Row) {
        // add one to where we delete, because we're increasing the index by inserting
        deleteAt += 1;
    } else {
        // add one to where we insert, because we haven't deleted the original yet
        insertAt += 1;
    }
    tableItems.Insert (insertAt, item);
    tableItems.RemoveAt (deleteAt);
}
```

Finally, to get the table into edit mode, the **Edit**
button needs to call `SetEditing` like this

```csharp
table.SetEditing (true, true);
```

and when the user is finished editing, the **Done**
button should turn editing mode off:

```csharp
table.SetEditing (false, true);
```

## Row Insertion Editing Style

Row insertion from within the table is an uncommon user interface – the
main example in the standard iOS apps is the **Edit
Contact** screen. This screenshot shows how the row insertion functionality
works – in edit mode there is an additional row that (when clicked) inserts
additional rows into the data. When editing is complete, the temporary **(add new)** row is removed.

 [![When editing is complete, the temporary add new row is removed](editing-images/image12.png)](editing-images/image12.png#lightbox)

There are a number of different methods on `UITableViewSource`
that affect a table’s edit mode behavior. These methods have been implemented
as follows in the example code:

- **EditingStyleForRow** – returns  `UITableViewCellEditingStyle.Delete` for the rows containing data, and returns  `UITableViewCellEditingStyle.Insert` for the last row (which will be added specifically to behave as an insert button). 
- **CustomizeMoveTarget** – While the user is moving a cell the return value from this optional method can override their choice of location. This means you can prevent them from ‘dropping’ the cell in certain positions – such as this example that prevents any row from being moved after the  **(add new)** row. 
- **CanMoveRow** – return true to enable the move ‘handle’ or false to prevent moving. In the example, the last row has the move ‘handle’ hidden because it is intended to server as an insert button only. 

We also add two custom methods to add the ‘insert’ row and then remove it
again when no longer required. They are called from the **Edit** and **Done** buttons:

- **WillBeginTableEditing** – When the  **Edit** button is touched it calls  `SetEditing` to put the table in edit mode. This triggers the WillBeginTableEditing method where we display the  **(add new)** row at the end of the table to act as an ‘insert button’. 
- **DidFinishTableEditing** – When the Done button is touched  `SetEditing` is called again to turn off edit mode. The example code removes the  **(add new)** row from the table when editing is no longer required. 

These method overrides are implemented in the sample file
**TableEditModeAdd/Code/TableSource.cs**:

```csharp
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    if (tableView.Editing) {
        if (indexPath.Row == tableView.NumberOfRowsInSection (0) - 1)
            return UITableViewCellEditingStyle.Insert;
        else
            return UITableViewCellEditingStyle.Delete;
    } else // not in editing mode, enable swipe-to-delete for all rows
        return UITableViewCellEditingStyle.Delete;
}
public override NSIndexPath CustomizeMoveTarget (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath proposedIndexPath)
{
    var numRows = tableView.NumberOfRowsInSection (0) - 1; // less the (add new) one
    if (proposedIndexPath.Row >= numRows)
        return NSIndexPath.FromRowSection(numRows - 1, 0);
    else
        return proposedIndexPath;
} 
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return indexPath.Row < tableView.NumberOfRowsInSection (0) - 1;
}
```

These two custom methods are used to add and remove the **(add
new)** row when the table’s editing mode is enabled or disabled:

```csharp
public void WillBeginTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // insert the 'ADD NEW' row at the end of table display
    tableView.InsertRows (new NSIndexPath[] { 
            NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0), 0) 
        }, UITableViewRowAnimation.Fade);
    // create a new item and add it to our underlying data (it is not intended to be permanent)
    tableItems.Add (new TableItem ("(add new)"));
    tableView.EndUpdates (); // applies the changes
}
public void DidFinishTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // remove our 'ADD NEW' row from the underlying data
    tableItems.RemoveAt ((int)tableView.NumberOfRowsInSection (0) - 1); // zero based :)
    // remove the row from the table display
    tableView.DeleteRows (new NSIndexPath[] { NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0) - 1, 0) }, UITableViewRowAnimation.Fade);
    tableView.EndUpdates (); // applies the changes
}
```

Finally, this code instantiates the **Edit** and **Done** buttons, with lambdas that enable or disable edit mode
when they’re touched:

```csharp
done = new UIBarButtonItem(UIBarButtonSystemItem.Done, (s,e)=>{
    table.SetEditing (false, true);
    NavigationItem.RightBarButtonItem = edit;
    tableSource.DidFinishTableEditing(table);
});
edit = new UIBarButtonItem(UIBarButtonSystemItem.Edit, (s,e)=>{
    if (table.Editing)
        table.SetEditing (false, true); // if we've half-swiped a row
    tableSource.WillBeginTableEditing(table);
    table.SetEditing (true, true);
    NavigationItem.LeftBarButtonItem = null;
    NavigationItem.RightBarButtonItem = done;
});
```

This row insertion UI pattern is not used very often, however you can also
use the `UITableView.BeginUpdates` and `EndUpdates` methods to animate the insertion
or removal of cells in any table. The rule for using those methods is that the
difference in value returned by `RowsInSection` between the `BeginUpdates` and
`EndUpdates` calls must match the net number of cells added/deleted with the
`InsertRows` and `DeleteRows` methods. If the underlying datasource isn’t changed
to match the insertions/deletions on the table view an error will occur.

## Related Links

- [WorkingWithTables (sample)](/samples/xamarin/ios-samples/workingwithtables)