---
title: "XAML Field Modifiers in Xamarin.Forms"
description: "The x:FieldModifier namespace attribute specifies the access level for generated fields for named XAML elements."
ms.prod: xamarin
ms.assetid: 12357CE0-3C11-4B62-947F-72DB6DFC23A2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2018
---

# XAML Field Modifiers in Xamarin.Forms

_The `x:FieldModifier` namespace attribute specifies the access level for generated fields for named XAML elements._

## Overview

Valid values of the attribute are:

- `Public` – specifies that the generated field for the XAML element is `public`.
- `NotPublic` – specifies that the generated field for the XAML element is `internal` to the assembly.

If the value of the attribute isn't set, the generated field for the element will be `private`.

The following conditions must be met for an `x:FieldModifier` attribute to be processed:

- The top-level XAML element must be a valid `x:Class`.
- The current XAML element has an `x:Name` specified.

The following XAML shows examples of setting the attribute:

```xaml
<Label x:Name="privateLabel" />
<Label x:Name="internalLabel" x:FieldModifier="NotPublic" />
<Label x:Name="publicLabel" x:FieldModifier="Public" />
```

> [!NOTE]
> The `x:FieldModifier` attribute cannot be used to specify the access level of a XAML class.
