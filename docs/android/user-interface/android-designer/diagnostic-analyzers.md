---
title: "Android Layout Diagnostics Analyzers"
description: "This guide lists all of the currently supported Android layout diagnostic analyzers on Xamarin.Android."
ms.prod: xamarin
ms.assetid: BD252EA7-7E69-4DB4-96AB-D52CC0510C8F
ms.technology: xamarin-android
author: decriptor
ms.author: stepsha
ms.date: 04/07/2020
---

# Android designer diagnostic analyzers

This guide lists all of the currently supported Android layout diagnostic analyzers.

## Accessibility

The following analyzers help to improve accessibility support:

| ID | Title | Severity | Description |
|----|-------|----------|-------------|
| ContentDescription | Image without `contentDescription` | Warning | Missing `contentDescription` attribute on image |

## Correctness

The following analyzers help fix correctness issues in a layout:

| ID | Title | Severity | Description | Help |
|----|-------|----------|-------------|------|
| AdapterViewChildren | AdapterView with children | Warning | AdapterViews cannot have children in XML | [Link](xref:Android.Widget.AdapterView) |
| MissingId | Fragments should specify an `id` or `tag` | Warning |This `<fragment>` tag should specify an `id` or a `tag` to preserve state across activity restarts | [Link](xref:Android.App.Fragment) |
| NestedScrollingVertical | Nested vertically scrolling elements | Warning | Nested scrolling widgets |
| NestedScrollingHorizontal | Nested horizontally scrolling elements | Warning | Nested scrolling widgets |
| ScrollViewSize | ScrollView children with wrong fill_parent/match_parent sizes | Warning | ScrollView children with wrong fill_parent/match_parent sizes |
| ScrollViewCount | ScrollViews can have only one child | Warning | A scroll view can have only one child |
| MissingAndroidNamespace | Missing Android namespace on attribute | Error | Missing Android XML namespace; your attribute will be interpreted as a custom attribute |
| DuplicateIDs | Duplicate IDs | Error | Duplicate ids within a single layout |
| IncludeLayoutParamsMissingWidthAndHeight | Missing both width and height | Error | Ignored layout params on include | [Link](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work) |
| IncludeLayoutParamsMissingWidth | Missing width | Error | Ignored layout params on include | [Link](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work) |
| IncludeLayoutParamsMissingHeight | Missing height | Error | Ignored layout params on include | [Link](https://stackoverflow.com/questions/2631614/does-android-xml-layouts-include-tag-really-work) |
| Orientation | Missing explicit orientation | Error | Missing explicit orientation |
| Suspicious0dp | Suspicious 0dp dimension | Error | Suspicious 0dp dimension |
| RequiredSizeWidth | Missing width attribute | Error | Missing attribute: layout_width |
| RequiredSizeHeight | Missing height attribute | Error | Missing attribute: layout_height |
| WebViewLayout | WebViews in wrap_content parents | Error |
| WrongCase | Wrong case for view tag | Error | Wrong case for view tag | [Link](xref:Android.App.Fragment) |

## Design

The following analyzers help to improve how you join layout files:

| ID | Title | Severity | Description |
|----|-------|----------|-------------|
| HardcodedColor | Hardcoded color | Info | Hardcoded color often leads to inconsistency |
| HardcodedSize  | Hardcoded size  | Info | Hardcoded size often leads to inconsistency  |
| HardcodedText  | Hardcoded text  | Warning | Hardcoded text |
| UnresolvedResource | Unresolved resource URL | Warning | This resource URL cannot be resolved |
| XmlErrors | XML syntax error | Error | XML syntax error |

## Performance

The following analyzers help improve the performance of your layout:

| ID | Title | Severity | Description |
|----|-------|----------|-------------|
| NestedWeights | Nested layout weights | Warning | Nested weights are bad for performance |
| TooManyViews | Layout has too many views | Warning | Layout has too many views |
| TooDeepLayout | Layout hierarchy is too deep | Warning | Layout hierarchy is too deep |
| UselessParent | Useless parent layout | Warning | Useless parent layout |
| UselessLeaf | Useless leaf layout | Warning | This `%1$s` view is useless (no children, no `background`, no `id`, no `style`) |

## Usability

The following analyzers help improve layout usability for your customers:

| ID | Title | Severity | Description |
|----|-------|----------|-------------|
| NegativeMargin | Negative Margins | Warning | Negative Margins |
| MissingInputType | EditText with no inputType | Warning | No input type specified |
| InputTypePhone | EditText appears to be a phone number | Warning | The view name suggests this is a phone number, but it does not include `phone` in the `inputType` |
| InputTypeNumber | EditText appears to be a number | Warning | The view name suggests this is a number, but it does not include a numeric `inputType` (such as `numberDecimal`) |
| InputTypePassword | EditText appears to be a password | Warning | The view name suggests this is a password, but it does not include `password` in the `inputType` (such as `textVisiblePassword`) |
| InputTypePIN | EditText appears to be a PIN | Warning | The view name suggests this is a password (PIN), but it does not include `numberPassword` in the `inputType` |
| InputTypeEmail | EditText appears to be an email | Warning | The view name suggests this is an e-mail address, but it does not include `email` in the `inputType` (such as `textEmailAddress`) |
| InputTypeURI | EditText appears to be a URI | Warning | The view name suggests this is a URI, but it does not include `textUri` in the `inputType` |
| InputTypeDate | EditText appears to be a date | Warning | The view name suggests this is a date, but it does not include `date` in the `inputType` (such as `datetime`) |
