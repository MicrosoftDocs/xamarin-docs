---
title: "Text in Xamarin.Forms"
description: "Xamarin.Forms has three primary views for working with text, and this article explains how to use them to enter and display text in Xamarin.Forms applications."
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2018
---

# Text in Xamarin.Forms

[![Download Sample](~/media/shared/download.png) Download the sample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Using Xamarin.Forms to enter or display text._

Xamarin.Forms has three primary views for working with text:

- **[Label](#Label)** &mdash; for presenting single or multi-line text. Can show text with multiple formatting options in the same line.
- **[Entry](#Entry)** &mdash; for entering text that is only one line. Entry has a password mode.
- **[Editor](#Editor)** &mdash; for entering text that could take more than one line.

Text appearance can be changed using built-in or custom [styles](#Styles) and some controls support custom [fonts](#Fonts).

<a name="Label" />

## [Label](label.md)

The `Label` view is used to display text. It can show multiple lines of text or a single line of text. `Label` can present text with multiple formatting options used in inline. The label view can wrap or truncate text when it can't fit on one line.

![](images/label.png "Label Example")

See the [Label](label.md) article for more detailed information.

For information on customizing the font used in a label, see [Fonts](fonts.md).

<a name="Entry" />

## [Entry](entry.md)

`Entry` is used to accept single-line text input. `Entry` offers control over colors and fonts. `Entry` has a password mode and can show placeholder text until text is entered.

![](images/entry.png "Entry Example")

See the [Entry](entry.md) article for more information.

Note that, unlike `Label`, `Entry` cannot have custom font settings.

<a name="Editor" />

## [Editor](editor.md)

`Editor` is used to accept multi-line text input. `Editor` offers control over colors and fonts.

![](images/editor.png "Editor Example")

See the [Editor](editor.md) article for more information.

<a name="Fonts" />

## [Fonts](fonts.md)

Many controls support different font settings using the built-in fonts on each platform, or custom fonts included with your app. See the [Fonts](fonts.md) article for more detailed information.

<a name="Styles" />

## [Styles](styles.md)

Refer to [working with styles](~/xamarin-forms/user-interface/styles/index.md) to learn how to set up font, [color](~/xamarin-forms/user-interface/colors.md), and other display properties that apply across multiple controls.

## Related Links

- [Text (sample)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
