---
title: "Working with Tables and Cells in Xamarin.iOS"
description: "This document links to various guides that describe how to display data with the UITableView control in a Xamarin.iOS app."
ms.service: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/06/2016
no-loc: [Objective-C]
---

# Working with Tables and Cells in Xamarin.iOS

> [!WARNING]
> The iOS Designer was deprecated in Visual Studio 2019 version 16.8 and Visual Studio 2019 for Mac version 8.8, and 
> removed in Visual Studio 2019 version 16.9 and Visual Studio for Mac version 8.9.
> The recommended way to build iOS user interfaces is directly on a Mac running Xcode's Interface Builder. For more information, see [Designing user interfaces with Xcode](~/ios/user-interface/storyboards/index.md). 

This section introduces the classes used to create and display tables then provides examples of how to use them in Xamarin.iOS. It will cover using the default appearance for tables, customizing the layout, implementing editing and using the Xamarin iOS Designer to design a table visually. Sometimes the display is obviously a list of rows (such as the Music app) and other times it is difficult to recognize the table control (such as editing in the Contacts app, or a conversation in Messages app).

For those working on cross-platform applications with Xamarin.Android, the UITableView control is similar to the ListView class in Android (and the UITableViewSource class is similar to Android’s Adapter classes).

These articles will take a comprehensive look at working with tables, including:

- **Table parts** – Introducing and explaining the visual elements of the  `UITableView` control. 
- **Displaying data in tables** – Demonstrating how to create and populate a table, use different table and cell styles and avoid memory issues by recycling cell objects. 
- **Advanced usage** – Building custom cells and using the editing features of the UITableView class. 
- **Creating a table visually** – Using the Xamarin Designer for iOS to create a table-driven interface with a Storyboard. 

## Contents

 [Table Parts &amp; Functionality](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Populating a Table with Data](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Customizing a Table's Appearance](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Editing](~/ios/user-interface/controls/tables/editing.md)

 [Row Actions](~/ios/user-interface/controls/tables/row-action.md)

 [Creating Tables in a Storyboard](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)

 [Auto-Sizing Row Height](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## Related Links

- [WorkingWithTables (sample)](/samples/xamarin/ios-samples/workingwithtables)
- [Tables in Storyboards (sample)](/samples/xamarin/ios-samples/storyboardtable)
- [Introduction to Storyboards](~/ios/user-interface/storyboards/index.md)
- [Storyboard a TableView Recipe](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [Introduction to MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [TableEditing sample on Github](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [TableParts sample on Github](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [TableAndCellStyles sample on Github](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [UITableView Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [UITableViewCell Class Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [UITableViewDataSource](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)