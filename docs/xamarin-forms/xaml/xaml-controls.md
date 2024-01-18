---
title: "XAML Controls"
description: "All of the views that are defined in Xamarin.Forms can be referenced from XAML files."
ms.topic: article
ms.service: xamarin
ms.assetid: 639BD392-1496-41BB-BB09-7652273AC9D8
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/09/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# XAML Controls

[![Download Sample](~/media/shared/download.png) Download the sample](/samples/xamarin/xamarin-forms-samples/formsgallery)

Views are user-interface objects such as labels, buttons, and sliders that are commonly known as *controls* or *widgets* in other graphical programming environments. The views supported by Xamarin.Forms all derive from the [`View`](xref:Xamarin.Forms.View) class.

All of the views that are defined in Xamarin.Forms can be referenced from XAML files.

## Views for presentation

| View | Example |
| --- | --- |
| <h3>BoxView</h3>Displays a rectangle of a particular color.<p align="center">![Screenshot of a BoxView](xaml-controls-images/BoxView.png "BoxView")</p>[API](xref:Xamarin.Forms.BoxView) / [Guide](~/xamarin-forms/user-interface/boxview.md) | <pre>&lt;BoxView Color="Accent"<br />         WidthRequest="150"<br />         HeightRequest="150"<br />         HorizontalOptions="Center"&gt;</pre> |
| <h3>Ellipse</h3>Displays an ellipse or circle.<p align="center">![Screenshot of an Ellipse](xaml-controls-images/Ellipse.png "Ellipse")</p>[API](xref:Xamarin.Forms.Shapes.Ellipse) / [Guide](~/xamarin-forms/user-interface/shapes/ellipse.md) | <pre>&lt;Ellipse Fill="Red"<br />         WidthRequest="150"<br />         HeightRequest="50"<br />         HorizontalOptions="Center" /&gt;</pre> |
| <h3>Image</h3>Displays a bitmap.<p align="center">![Screenshot of an Image](xaml-controls-images/Image.png "Image")</p>[API](xref:Xamarin.Forms.Image) / [Guide](~/xamarin-forms/user-interface/images.md) | <pre>&lt;Image Source="https://aka.ms/campus.jpg"<br />       Aspect="AspectFit"<br />       HorizontalOptions="Center" /&gt;</pre></p> |
| <h3>Label</h3>Displays one or more lines of text.<p align="center">![Screenshot of a Label](xaml-controls-images/Label.png "Label")</p>[API](xref:Xamarin.Forms.Label) / [Guide](~/xamarin-forms/user-interface/text/label.md) | <pre>&lt;Label Text="Hello, Xamarin.Forms!"<br />       FontSize="Large"<br />       FontAttributes="Italic"<br />       HorizontalTextAlignment="Center" /&gt;</pre> |
| <h3>Line</h3>Display a line.<p align="center">![Screenshot of a Line](xaml-controls-images/Line.png "Line")</p>[API](xref:Xamarin.Forms.Shapes.Line) / [Guide](~/xamarin-forms/user-interface/shapes/line.md) | <pre>&lt;Line X1="40"<br />      Y1="0"<br />      X2="0"<br />      Y2="120"<br />      Stroke="Red"<br />      HorizontalOptions="Center" /&gt;</pre> |
| <h3>Map</h3>Displays a map.<p align="center">![Screenshot of a Map](xaml-controls-images/Map.png "Map")</p>[API](xref:Xamarin.Forms.Maps.Map) / [Guide](~/xamarin-forms/user-interface/map/index.md) | <pre>&lt;maps:Map ItemsSource="{Binding Locations}" /&gt;</pre> |
| <h3>Path</h3>Display curves and complex shapes.<p align="center">![Screenshot of a Path](xaml-controls-images/Path.png "Path")</p>[API](xref:Xamarin.Forms.Shapes.Path) / [Guide](~/xamarin-forms/user-interface/shapes/path.md) | <pre>&lt;Path Stroke="Black"<br />      Aspect="Uniform"<br />      HorizontalOptions="Center"<br />      HeightRequest="100"<br />      WidthRequest="100"<br />      Data="M13.9,16.2<br />            L32,16.2 32,31.9 13.9,30.1Z<br />            M0,16.2<br />            L11.9,16.2 11.9,29.9 0,28.6Z<br />            M11.9,2<br />            L11.9,14.2 0,14.2 0,3.3Z<br />            M32,0<br />            L32,14.2 13.9,14.2 13.9,1.8Z" /&gt;</pre> |
| <h3>Polygon</h3>Display a polygon.<p align="center">![Screenshot of a Polygon](xaml-controls-images/Polygon.png "Polygon")</p>[API](xref:Xamarin.Forms.Shapes.Polygon) / [Guide](~/xamarin-forms/user-interface/shapes/polygon.md) | <pre>&lt;Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96,<br/>                 50 96, 48 192, 150 200 144 48"<br />         Fill="Blue"<br />         Stroke="Red"<br />         StrokeThickness="3"<br />         HorizontalOptions="Center" /&gt;</pre> |
| <h3>Polyline</h3>Display a series of connected straight lines.<p align="center">![Screenshot of a Polyline](xaml-controls-images/Polyline.png "Polyline")</p>[API](xref:Xamarin.Forms.Shapes.Polyline) / [Guide](~/xamarin-forms/user-interface/shapes/Polyline.md) | <pre>&lt;Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0<br />                  43,60 48,30 100,30"<br />          Stroke="Red"<br />          HorizontalOptions="Center" /&gt;</pre> |
| <h3>Rectangle</h3>Display a rectangle or square.<p align="center">![Screenshot of a Rectangle](xaml-controls-images/Rectangle.png "Rectangle")</p>[API](xref:Xamarin.Forms.Shapes.Rectangle) / [Guide](~/xamarin-forms/user-interface/shapes/rectangle.md) | <pre>&lt;Rectangle Fill="Red"<br />           WidthRequest="150"<br />           HeightRequest="50"<br />           HorizontalOptions="Center" /&gt;</pre> |  
| <h3>WebView</h3>Displays Web pages or HTML content.<p align="center">![Screenshot of a WebView](xaml-controls-images/WebView.png "WebView")</p>[API](xref:Xamarin.Forms.WebView) / [Guide](~/xamarin-forms/user-interface/webview.md) | <pre>&lt;WebView Source="https://learn.microsoft.com/xamarin/"<br/>         VerticalOptions="FillAndExpand" /&gt;</pre> |
|     |     |

## Views that initiate commands

| View | Example |
| --- | --- |
| <h3>Button</h3>Displays text in a rectangular object.<p align="center">![Screenshot of a Button](xaml-controls-images/Button.png "Button")</p>[API](xref:Xamarin.Forms.Button) / [Guide](~/xamarin-forms/user-interface/button.md) | <pre>&lt;Button Text="Click Me!"<br />        Font="Large"<br />        BorderWidth="1"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand"<br />        Clicked="OnButtonClicked" /&gt;</pre> |
| <h3>ImageButton</h3>Displays an image in a rectangular object.<p align="center">![Screenshot of an ImageButton](xaml-controls-images/ImageButton.png "ImageButton")</p>[API](xref:Xamarin.Forms.ImageButton) / [Guide](~/xamarin-forms/user-interface/imagebutton.md) | <pre>&lt;ImageButton Source="XamarinLogo.png"<br />             HorizontalOptions="Center"<br />             VerticalOptions="CenterAndExpand"<br />             Clicked="OnImageButtonClicked" /&gt;</pre> |
| <h3>RadioButton</h3>Allows the selection of one option from a set.<p align="center">![Screenshot of a RadioButton](xaml-controls-images/RadioButton.png "RadioButton")</p>[Guide](~/xamarin-forms/user-interface/radiobutton.md) | <pre>&lt;RadioButton Text="Pineapple"<br/>             CheckedChanged="OnRadioButtonCheckedChanged" /&gt;</pre> |
| <h3>RefreshView</h3>Provides pull-to-refresh functionality for scrollable content.<p align="center">![Screenshot of a RefreshView](xaml-controls-images/RefreshView.png "RefreshView")</p>[Guide](~/xamarin-forms/user-interface/refreshview.md) | <pre>&lt;RefreshView IsRefreshing="{Binding IsRefreshing}"<br />             Command="{Binding RefreshCommand}" &gt;<br />    &lt;!-- Scrollable control goes here --&gt;<br />&lt;/RefreshView&gt;</pre> |
| <h3>SearchBar</h3> Accepts user input that it uses to perform a search.<p align="center">![Screenshot of a SearchBar](xaml-controls-images/SearchBar.png "SearchBar")</p>[Guide](~/xamarin-forms/user-interface/searchbar.md) | <pre>&lt;SearchBar Placeholder="Enter search term"<br />           SearchButtonPressed="OnSearchBarButtonPressed" /&gt;</pre> |
| <h3>SwipeView</h3> Provides context menu items that are revealed by a swipe gesture.<p align="center">![Screenshot of a SwipeView](xaml-controls-images/SwipeView.png "SwipeView")</p>[Guide](~/xamarin-forms/user-interface/swipeview.md) | <pre>&lt;SwipeView&gt;<br />    &lt;SwipeView.LeftItems&gt;<br />        &lt;SwipeItems&gt;<br />            &lt;SwipeItem Text="Delete"<br />                       IconImageSource="delete.png"<br />                       BackgroundColor="LightPink"<br />                       Invoked="OnDeleteInvoked" /&gt;<br />        &lt;/SwipeItems&gt;<br />    &lt;/SwipeView.LeftItems&gt;<br />    &lt;!-- Content --&gt;<br />&lt;/SwipeView&gt;</pre> |
|     |     |

## Views for setting values

| View | Example |
| --- | --- |
| <h3>CheckBox</h3>Allows the selection of a `boolean` value.<p align="center">![Screenshot of a CheckBox](xaml-controls-images/CheckBox.png "CheckBox")</p> [Guide](~/xamarin-forms/user-interface/checkbox.md) |<pre>&lt;CheckBox IsChecked="true"<br />          HorizontalOptions="Center"<br />          VerticalOptions="CenterAndExpand" /&gt;</pre>|
| <h3>Slider</h3>Allows the selection of a `double` value from a continuous range.<p align="center">![Screenshot of a Slider](xaml-controls-images/Slider.png "Slider")</p>[API](xref:Xamarin.Forms.Slider) / [Guide](~/xamarin-forms/user-interface/slider.md) | <pre>&lt;Slider Minimum="0"<br />        Maximum="100"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre> |
| <h3>Stepper</h3>Allows the selection of a `double` value from an incremental range.![Screenshot of a Stepper](xaml-controls-images/Stepper.png "Stepper")</p>[API](xref:Xamarin.Forms.Stepper) / [Guide](~/xamarin-forms/user-interface/stepper.md) | <pre>&lt;Stepper Minimum="0"<br />         Maximum="10"<br />         Increment="0.1"<br />         HorizontalOptions="Center"<br />         VerticalOptions="CenterAndExpand" /&gt;</pre> |
| <h3>Switch</h3>Allows the selection of a `boolean` value.<p align="center">![Screenshot of a Switch](xaml-controls-images/Switch.png "Switch")</p>[API](xref:Xamarin.Forms.Switch) / [Guide](~/xamarin-forms/user-interface/switch.md)| <pre>&lt;Switch IsToggled="false"<br />        HorizontalOptions="Center"<br />        VerticalOptions="CenterAndExpand" /&gt;</pre> |
| <h3>DatePicker</h3>Allows the selection of a date.<p align="center">![Screenshot of a DatePicker](xaml-controls-images/DatePicker.png "DatePicker")</p>[API](xref:Xamarin.Forms.DatePicker) / [Guide](~/xamarin-forms/user-interface/datepicker.md) | <pre>&lt;DatePicker Format="D"<br/>            VerticalOptions="CenterAndExpand" /&gt;</pre> |
| <h3>TimePicker</h3>Allows the selection of a time.<p align="center">![Screenshot of a TimePicker](xaml-controls-images/TimePicker.png "TimePicker")</p>[API](xref:Xamarin.Forms.TimePicker) / [Guide](~/xamarin-forms/user-interface/timepicker.md) | <pre>&lt;TimePicker Format="T"<br />            VerticalOptions="CenterAndExpand" /&gt;</pre> |
|     |     |

## Views for editing text

| View | Example |
| --- | --- |
| <h3>Entry</h3>Allows a single line of text to be entered and edited.<p align="center">![Screenshot of an Entry](xaml-controls-images/Entry.png "Entry")</p>[API](xref:Xamarin.Forms.Entry) / [Guide](~/xamarin-forms/user-interface/text/entry.md) | <<pre>&lt;Entry Keyboard="Email"<br />       Placeholder="Enter email address"<br />       VerticalOptions="CenterAndExpand" /&gt;</pre> |
| <h3>Editor</h3>Allows multiple lines of text to be entered and edited.<p align="center">![Screenshot of an Editor](xaml-controls-images/Editor.png "Label")</p>[API](xref:Xamarin.Forms.Editor) / [Guide](~/xamarin-forms/user-interface/text/editor.md) | <pre>&lt;Editor VerticalOptions="FillAndExpand" /&gt;</pre> |
|     |     |

## Views to indicate activity

| View | Example |
| --- | --- |
| <h3>ActivityIndicator</h3>Displays an animation to show that the application is engaged in a lengthy activity, without giving any indication of progress.<p align="center">![Screenshot of an ActivityIndicator](xaml-controls-images/ActivityIndicator.png "ActivityIndicator")</p>[API](xref:Xamarin.Forms.ActivityIndicator) / [Guide](~/xamarin-forms/user-interface/activityindicator.md) | <pre>&lt;ActivityIndicator IsRunning="True"<br />                   VerticalOptions="CenterAndExpand" /&gt;</pre> |
| <h3>ProgressBar</h3>Displays an animation to show that the application is progressing through a lengthy activity.<p align="center">![Screenshot of a ProgressBar](xaml-controls-images/ProgressBar.png "ProgressBar")</p>[API](xref:Xamarin.Forms.ProgressBar) / [Guide](~/xamarin-forms/user-interface/progressbar.md) | <pre>&lt;ProgressBar Progress=".5"<br />             VerticalOptions="CenterAndExpand" /&gt;</pre> |
|     |     |

## Views that display collections

| View | Example |
| --- | --- |
| <h3>CarouselView</h3>Displays a scrollable list of data items.<p align="center">![Screenshot of a CarouselView](xaml-controls-images/CarouselView.png "CarouselView")</p>[Guide](~/xamarin-forms/user-interface/carouselview/index.md) | <pre>&lt;CarouselView ItemsSource="{Binding Monkeys}"&gt;<br/>              ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre>|
| <h3>CollectionView</h3>Displays a scrollable list of selectable data items, using different layout specifications.<p align="center">![Screenshot of a CollectionView](xaml-controls-images/CollectionView.png "CollectionView")</p>[Guide](~/xamarin-forms/user-interface/collectionview/index.md) | <pre>&lt;CollectionView ItemsSource="{Binding Monkeys}"&gt;<br/>                ItemTemplate="{StaticResource MonkeyTemplate}"<br />                ItemsLayout="VerticalGrid, 2" /&gt;</pre> |
| <h3>IndicatorView</h3>Displays indicators that represent the number of items in a `CarouselView`.<p align="center">![Screenshot of an IndicatorView](xaml-controls-images/IndicatorView.png "IndicatorView")</p>[Guide](~/xamarin-forms/user-interface/indicatorview.md) | <pre>&lt;IndicatorView x:Name="indicatorView"<br />               IndicatorColor="LightGray"<br />               SelectedIndicatorColor="DarkGray" /&gt;</pre> |
| <h3>ListView</h3>Displays a scrollable list of selectable data items.<p align="center">![Screenshot of a ListView](xaml-controls-images/ListView.png "ListView")</p>[API](xref:Xamarin.Forms.ListView) / [Guide](~/xamarin-forms/user-interface/listview/index.md) | <pre>&lt;ListView ItemsSource="{Binding Monkeys}"&gt;<br />          ItemTemplate="{StaticResource MonkeyTemplate}" /&gt;</pre> |
| <h3>Picker</h3>Displays a select item from a list of text strings.<p align="center">![Screenshot of a Picker](xaml-controls-images/Picker.png "Picker")</p>[API](xref:Xamarin.Forms.Picker) / [Guide](~/xamarin-forms/user-interface/picker/index.md) | <<pre>&lt;Picker Title="Select a monkey"<br />        TitleColor="Red"&gt;<br />  &lt;Picker.ItemsSource&gt;<br />    &lt;x:Array Type="{x:Type x:String}"&gt;<br />      &lt;x:String&gt;Baboon&lt;/x:String&gt;<br />      &lt;x:String&gt;Capuchin Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Blue Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Squirrel Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Golden Lion Tamarin&lt;/x:String&gt;<br />      &lt;x:String&gt;Howler Monkey&lt;/x:String&gt;<br />      &lt;x:String&gt;Japanese Macaque&lt;/x:String&gt;<br />    &lt;/x:Array&gt;<br />  &lt;/Picker.ItemsSource&gt;<br />&lt;/Picker&gt;</pre> |
| <h3>TableView</h3>Displays a list of interactive rows.<p align="center">![Screenshot of a TableView](xaml-controls-images/TableView.png "TableView")</p>[API](xref:Xamarin.Forms.TableView) / [Guide](~/xamarin-forms/user-interface/tableview.md) | <pre>&lt;TableView Intent="Settings"&gt;<br />    &lt;TableRoot&gt;<br />        &lt;TableSection Title="Ring"&gt;<br />            &lt;SwitchCell Text="New Voice Mail" /&gt;<br />            &lt;SwitchCell Text="New Mail" On="true" /&gt;<br />        &lt;/TableSection&gt;<br />    &lt;/TableRoot&gt;<br />&lt;/TableView&gt;</pre> |
|     |     |

## Related links

- [Xamarin.Forms FormsGallery sample](/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms Samples](/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Xamarin.Forms API Documentation](/dotnet/api/xamarin.forms?view=xamarin-forms&preserve-view=true)
