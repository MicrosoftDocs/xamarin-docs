---
title: "XAML Standard (Preview) Controls"
description: "How to get started exploring the XAML Standard Preview in Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
---

# XAML Standard (Preview) Controls

![Preview](~/media/shared/preview.png)

This page lists the XAML Standard controls available in the
Preview, alongside their equivalent Xamarin.Forms control.

There is also a list of controls that have new property and
enumeration names in XAML Standard.

## Controls

<table style="width:300px">
  <tr><th>Xamarin.Forms</th><th>XAML Standard</th></tr>
  <tr><td>Frame</td><td>Border</td></tr>
  <tr><td>Picker</td><td>ComboBox</td></tr>
  <tr><td>ActivityIndicator</td><td>ProgressRing</td></tr>
  <tr><td>StackLayout</td><td>StackPanel</td></tr>
  <tr><td>Label</td><td>TextBlock</td></tr>
  <tr><td>Entry</td><td>TextBox</td></tr>
  <tr><td>Switch</td><td>ToggleSwitch</td></tr>
  <tr><td>ContentView</td><td>UserControl</td></tr>
</table>

## Properties and Enumerations

<table>
  <tr><th>Xamarin.Forms<br/>Controls with updated properties</th><th>Xamarin.Forms<br/>Property or Enum</th><th>XAML Standard<br/>Equivalent</th></tr>
  <tr><td>Button, Entry, Label, DatePicker, Editor, SearchBar, TimePicker</td><td>TextColor</td><td>Foreground</td></tr>
  <tr><td>VisualElement</td><td>BackgroundColor</td><td><i>Background *</i></td></tr>
  <tr><td>Picker, Button</td><td>BorderColor, OutlineColor</td><td>BorderBrush</td></tr>
  <tr><td>Button</td><td>BorderWidth</td><td>BorderThickness</td></tr>
  <tr><td>ProgressBar</td><td>Progress</td><td>Value</td></tr>
  <tr><td>Button, Entry, Label, Editor, SearchBar, Span, Font</td><td>FontAttributes<br/>Bold, Italic, None</td><td>FontStyle<br/>Italic, Normal</td></tr>
  <tr><td>Button, Entry, Label, Editor, SearchBar, Span, Font</td><td>FontAttributes</td><td><i>FontWeights *</i><br/>Bold, Normal</td></tr>
  <tr><td>InputView</td><td>Keyboard<br/>Default, Url, Number, Telephone, Text, Chat, Email</td><td><i>InputScopeNameValue *</i><br/>Default, Url, Number, TelephoneNumber, Text, Chat, EmailNameOrAddress</td></tr>
  <tr><td>StackPanel</td><td>StackOrientation</td><td><i>Orientation *</i></td></tr>
</table>

> [!IMPORTANT]
> Items marked with * are incomplete in the current preview


## Related Links

- [Preview NuGet](https://aka.ms/xf-xamlstandard-nuget)
