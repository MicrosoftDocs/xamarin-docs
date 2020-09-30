---
ms.assetid: 4D47185C-8998-4903-AE64-7E2A67F9DF7A
title: "UI Controls Comparison"
description: "This document provides a comparison of UI controls between Xamarin.Forms, Windows Forms, and WPF. It also links to other documentation that compares WPF to Xamarin.Forms."
author: davidortinau
ms.author: daortin
ms.date: 04/26/2017
---

# UI Controls Comparison

Below is a comparison of Xamarin.Forms controls with Windows Forms and WPF,
based on [this table](/dotnet/framework/wpf/advanced/windows-forms-controls-and-equivalent-wpf-controls).

Read more about the [similarities and differences between WPF and Xamarin.Forms](wpf.md)
to help update your desktop knowledge for mobile app development.

|Windows Forms|WPF|Xamarin.Forms|
|--- |--- |--- |
|[BindingNavigator](/dotnet/api/system.windows.forms.bindingnavigator)|-|-|
|[BindingSource](/dotnet/api/system.windows.forms.bindingsource)|[CollectionViewSource](/dotnet/api/system.windows.data.collectionviewsource)|Binding property, eg. BindingContext|
|[Button](/dotnet/api/system.windows.forms.button)|[Button](/dotnet/api/system.windows.controls.button)|Button|
|[CheckBox](/dotnet/api/system.windows.forms.checkbox)|[CheckBox](/dotnet/api/system.windows.controls.checkbox)|Switch|
|[CheckedListBox](/dotnet/api/system.windows.forms.checkedlistbox)|[ListBox](/dotnet/api/system.windows.controls.listbox) with composition.|ListView with composition.|
|[ColorDialog](/dotnet/api/system.windows.forms.colordialog)|-|-|
|[ComboBox](/dotnet/api/system.windows.forms.combobox)|[ComboBox](/dotnet/api/system.windows.controls.combobox) (does not support auto-complete)|Picker|
|[ContextMenuStrip](/dotnet/api/system.windows.forms.contextmenustrip)|[ContextMenu](/dotnet/api/system.windows.controls.contextmenu)|-|
|[DataGridView](/dotnet/api/system.windows.forms.datagridview)|[DataGrid](/dotnet/api/system.windows.controls.datagrid)|-|
|[DateTimePicker](/dotnet/api/system.windows.forms.datetimepicker)|[DatePicker](/dotnet/api/system.windows.controls.datepicker)|DatePicker & TimePicker|
|[DomainUpDown](/dotnet/api/system.windows.forms.domainupdown)|[TextBox](/dotnet/api/system.windows.controls.textbox) and two [RepeatButton](/dotnet/api/system.windows.controls.primitives.repeatbutton) controls.|Stepper|
|[ErrorProvider](/dotnet/api/system.windows.forms.errorprovider)|-|-|
|[FlowLayoutPanel](/dotnet/api/system.windows.forms.flowlayoutpanel)|[WrapPanel](/dotnet/api/system.windows.controls.wrappanel) or [StackPanel](/dotnet/api/system.windows.controls.stackpanel)|StackLayout or FlexLayout|
|[FolderBrowserDialog](/dotnet/api/system.windows.forms.folderbrowserdialog)|-|-|
|[FontDialog](/dotnet/api/system.windows.forms.fontdialog)|-|-|
|[Form](/dotnet/api/system.windows.forms.form)|[Window](/dotnet/api/system.windows.window)|Page|
|[GroupBox](/dotnet/api/system.windows.forms.groupbox)|[GroupBox](/dotnet/api/system.windows.controls.groupbox)|-|
|[HelpProvider](/dotnet/api/system.windows.forms.helpprovider)|No equivalent control (use ToolTips).|-|
|[HScrollBar](/dotnet/api/system.windows.forms.hscrollbar)|[ScrollBar](/dotnet/api/system.windows.controls.primitives.scrollbar) (Scrolling is built into container controls)|use ScrollView|
|[ImageList](/dotnet/api/system.windows.forms.imagelist)|-|-|
|[Label](/dotnet/api/system.windows.forms.label)|[Label](/dotnet/api/system.windows.controls.label)|Label|
|[LinkLabel](/dotnet/api/system.windows.forms.linklabel)|No equivalent control (you can use the [Hyperlink](/dotnet/api/system.windows.documents.hyperlink) class to host hyperlinks within flow content).|-|
|[ListBox](/dotnet/api/system.windows.forms.listbox)|[ListBox](/dotnet/api/system.windows.controls.listbox)|Use ListView|
|[ListView](/dotnet/api/system.windows.forms.listview)|[ListView](/dotnet/api/system.windows.controls.listview)|ListView|
|[MaskedTextBox](/dotnet/api/system.windows.forms.maskedtextbox)|-|-|
|[MenuStrip](/dotnet/api/system.windows.forms.menustrip)|[Menu](/dotnet/api/system.windows.controls.menu)|Consider MasterDetailPage or TabbedPage|
|[MonthCalendar](/dotnet/api/system.windows.forms.monthcalendar)|[Calendar](/dotnet/api/system.windows.controls.calendar)|-|
|[NotifyIcon](/dotnet/api/system.windows.forms.notifyicon)|-|-|
|[NumericUpDown](/dotnet/api/system.windows.forms.numericupdown)|[TextBox](/dotnet/api/system.windows.controls.textbox) and two [RepeatButton](/dotnet/api/system.windows.controls.primitives.repeatbutton) controls.|Stepper|
|[OpenFileDialog](/dotnet/api/system.windows.forms.openfiledialog)|[OpenFileDialog](/dotnet/api/microsoft.win32.openfiledialog)|-|
|[PageSetupDialog](/dotnet/api/system.windows.forms.pagesetupdialog)|-|-|
|[Panel](/dotnet/api/system.windows.forms.panel)|[Canvas](/dotnet/api/system.windows.controls.canvas)|View or AbsoluteLayout|
|[PictureBox](/dotnet/api/system.windows.forms.picturebox)|[Image](/dotnet/api/system.windows.controls.image)|Image|
|[PrintDialog](/dotnet/api/system.windows.forms.printdialog)|[PrintDialog](/dotnet/api/system.windows.controls.printdialog)|-|
|[PrintDocument](/dotnet/api/system.drawing.printing.printdocument)|-|-|
|[PrintPreviewControl](/dotnet/api/system.windows.forms.printpreviewcontrol)|[DocumentViewer](/dotnet/api/system.windows.controls.documentviewer)|-|
|[PrintPreviewDialog](/dotnet/api/system.windows.forms.printpreviewdialog)|-|-|
|[ProgressBar](/dotnet/api/system.windows.forms.progressbar)|[ProgressBar](/dotnet/api/system.windows.controls.progressbar)|ProgressBar|
|[PropertyGrid](/dotnet/api/system.windows.forms.propertygrid)|-|-|
|[RadioButton](/dotnet/api/system.windows.forms.radiobutton)|[RadioButton](/dotnet/api/system.windows.controls.radiobutton)|-|
|[RichTextBox](/dotnet/api/system.windows.forms.richtextbox)|[RichTextBox](/dotnet/api/system.windows.controls.richtextbox)|Editor does not support rich (formatted) text, Entry for single line text|
|[SaveFileDialog](/dotnet/api/system.windows.forms.savefiledialog)|[SaveFileDialog](/dotnet/api/microsoft.win32.savefiledialog)|-|
|[ScrollableControl](/dotnet/api/system.windows.forms.scrollablecontrol)|[ScrollViewer](/dotnet/api/system.windows.controls.scrollviewer)|ScrollView|
|[SoundPlayer](/dotnet/api/system.media.soundplayer)|[MediaPlayer](/dotnet/api/system.windows.media.mediaplayer)|-|
|[SplitContainer](/dotnet/api/system.windows.forms.splitcontainer)|[GridSplitter](/dotnet/api/system.windows.controls.gridsplitter)|Consider MasterDetailPage|
|[StatusStrip](/dotnet/api/system.windows.forms.statusstrip)|[StatusBar](/dotnet/api/system.windows.controls.primitives.statusbar)|-|
|[TabControl](/dotnet/api/system.windows.forms.tabcontrol)|[TabControl](/dotnet/api/system.windows.controls.tabcontrol)|TabbedPage|
|[TableLayoutPanel](/dotnet/api/system.windows.forms.tablelayoutpanel)|[Grid](/dotnet/api/system.windows.controls.grid)|Grid|
|[TextBox](/dotnet/api/system.windows.forms.textbox)|[TextBox](/dotnet/api/system.windows.controls.textbox)|Editor does not support rich (formatted) text|
|[Timer](/dotnet/api/system.windows.forms.timer)|[DispatcherTimer](/dotnet/api/system.windows.threading.dispatchertimer)|Device.StartTime()|
|[ToolStrip](/dotnet/api/system.windows.forms.toolstrip)|[ToolBar](/dotnet/api/system.windows.controls.toolbar)|Page.ToolbarItems and ToolbarItem|
|[ToolStripContainer](/dotnet/api/system.windows.forms.toolstripcontainer), [ToolStripDropDown](/dotnet/api/system.windows.forms.toolstripdropdown), [ToolStripDropDownMenu](/dotnet/api/system.windows.forms.toolstripdropdownmenu), [ToolStripPanel](/dotnet/api/system.windows.forms.toolstrippanel)|[ToolBar](/dotnet/api/system.windows.controls.toolbar) with composition.|Page.ToolbarItems and ToolbarItem with composition|
|[ToolTip](/dotnet/api/system.windows.forms.tooltip)|[ToolTip](/dotnet/api/system.windows.controls.tooltip)|Use Accessibility features|
|[TrackBar](/dotnet/api/system.windows.forms.trackbar)|[Slider](/dotnet/api/system.windows.controls.slider)|Slider|
|[TreeView](/dotnet/api/system.windows.forms.treeview)|[TreeView](/dotnet/api/system.windows.controls.treeview)|Consider hierarchical ListView in a NavigationPage|
|[UserControl](/dotnet/api/system.windows.forms.usercontrol)|[UserControl](/dotnet/api/system.windows.controls.usercontrol)|View and also Custom Renderers|
|[VScrollBar](/dotnet/api/system.windows.forms.vscrollbar)|[ScrollBar](/dotnet/api/system.windows.controls.primitives.scrollbar)|use ScrollView|
|[WebBrowser](/dotnet/api/system.windows.forms.webbrowser)|[WebBrowser](/dotnet/api/system.windows.controls.webbrowser)|WebView|