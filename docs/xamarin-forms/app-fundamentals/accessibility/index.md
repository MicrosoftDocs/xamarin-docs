---
title: "Xamarin.Forms Accessibility"
description: "Building an accessible application ensures that the application is usable by people who approach the user interface with a range of needs and experiences."
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/28/2019
no-loc: [Xamarin.Forms, Xamarin.Essentials]
ms.custom: video
---

# Xamarin.Forms Accessibility

_Building an accessible application ensures that the application is usable by people who approach the user interface with a range of needs and experiences._

Making a Xamarin.Forms application accessible means thinking about the layout and design of many user interface elements. For guidelines on issues to consider, see the [Accessibility Checklist](~/cross-platform/app-fundamentals/accessibility.md). Many accessibility concerns such as large fonts, and suitable color and contrast settings can already be addressed by Xamarin.Forms APIs.

The [Android accessibility](~/android/app-fundamentals/accessibility.md) and [iOS accessibility](~/ios/app-fundamentals/accessibility.md) guides contain details of the native APIs exposed by Xamarin, and the [UWP accessibility guide on MSDN](/windows/uwp/design/accessibility/basic-accessibility-information) explains the native approach on that platform. These APIs are used to fully implement accessible applications on each platform.

Xamarin.Forms does not currently have *built-in* support for all of the accessibility APIs available on each of the underlying platforms. However, it does support setting automation properties on user interface elements to support screen reader and navigation assistance tools, which is one of the most important parts of building accessible applications. For more information, see [Automation Properties](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md).

Xamarin.Forms applications can also have the tab order of controls specified, to improve usability and accessibility. For more information, see [Keyboard Accessibility](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md).

Other accessibility APIs (such as [PostNotification on iOS](~/ios/app-fundamentals/accessibility.md)) may be better suited to a [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md) or [Custom Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) implementation. These are not covered in this guide.

## Testing Accessibility

Xamarin.Forms applications typically target multiple platforms, which means testing the accessibility features according to the platform. Follow these links to learn how to test accessibility on each platform:

- [**iOS Testing**](~/ios/app-fundamentals/accessibility.md)
- [**Android Testing**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)**](/windows/win32/winauto/accscope)

## Related Links

- [Cross-platform Accessibility](~/cross-platform/app-fundamentals/accessibility.md)
- [Automation Properties](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)
- [Keyboard Accessibility](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)

## Related Video

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Making-Mobile-Apps-Accessible/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]