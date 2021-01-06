---
title: "Xamarin.Forms Shell Custom Renderers"
description: "Xamarin.Forms Shell applications are highly customizable through the properties and methods that the various Shell classes expose. However, it's also possible to create a Shell custom renderer when more sophisticated platform-specific customizations are required."
ms.prod: xamarin
ms.assetid: 3B1A6AE8-1D1E-4C34-B9AB-48F4444FEF32
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/29/2020
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Shell Custom Renderers

One of the advantages of Xamarin.Forms Shell applications is that their appearance and behavior is highly customizable through the properties and methods that the various Shell classes expose. However, it's also possible to create a Shell custom renderer when more sophisticated platform-specific customizations are required. As with other custom renderers, a Shell custom renderer can be added to just one platform project to customize appearance and behavior, while allowing the default behavior on the other platform; or a different Shell custom renderer can be added to each platform project to customize appearance and behavior on both iOS and Android.

Shell applications are rendered using the `ShellRenderer` class on iOS and Android. On iOS, the `ShellRenderer` class can be found in the `Xamarin.Forms.Platform.iOS` namespace. On Android, the `ShellRenderer` class can be found in the `Xamarin.Forms.Platform.Android` namespace.

The process for creating a Shell custom renderer is as follows:

1. Subclass the `Shell` class. This will already be accomplished in your Shell application.
1. Consume the subclassed `Shell` class. This will already be accomplished in your Shell application.
1. Create a custom renderer class that derives from the `ShellRenderer` class, on the required platforms.

## Create a custom renderer class

The process for creating a Shell custom renderer class is as follows:

1. Create a subclass of the `ShellRenderer` class.
1. Override the required methods to perform the required customization.
1. Add an `ExportRendererAttribute` to the `ShellRenderer` subclass, to specify that it will be used to render the Shell application. This attribute is used to register the custom renderer with Xamarin.Forms.

> [!NOTE]
> It's optional to provide a Shell custom renderer in each platform project. If a custom renderer isn't registered, then the default `ShellRenderer` class will be used.

The `ShellRenderer` class exposes the following overridable methods:

| iOS | Android | UWP |
| --- | --- | --- |
| `SetElementSize`<br />`CreateFlyoutRenderer`<br />`CreateNavBarAppearanceTracker`<br />`CreatePageRendererTracker`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellItemTransition`<br />`CreateShellSearchResultsRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTabBarAppearanceTracker`<br />`Dispose`<br />`OnCurrentItemChanged`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`UpdateBackgroundColor` | `CreateFragmentForPage`<br />`CreateShellFlyoutContentRenderer`<br />`CreateShellFlyoutRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellSectionRenderer`<br />`CreateTrackerForToolbar`<br />`CreateToolbarAppearanceTracker`<br />`CreateTabLayoutAppearanceTracker`<br />`CreateBottomNavViewAppearanceTracker`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`SwitchFragment`<br />`Dispose` | `CreateShellFlyoutTemplateSelector`<br />`CreateShellHeaderRenderer`<br />`CreateShellItemRenderer`<br />`CreateShellSectionRenderer`<br />`OnElementPropertyChanged`<br />`OnElementSet`<br />`UpdateFlyoutBackdropColor`<br />`UpdateFlyoutBackgroundColor` |

The `FlyoutItem` and `TabBar` classes are aliases for the `ShellItem` class, and the `Tab` class is an alias for the `ShellSection` class. Therefore, the `CreateShellItemRenderer` method should be overridden when creating a custom renderer for `FlyoutItem` objects, and the `CreateShellSectionRenderer` method should be overridden when creating a custom renderer for `Tab` objects.

> [!IMPORTANT]
> There are additional Shell renderer classes, such as `ShellSectionRenderer` and `ShellItemRenderer`, on iOS, Android, and UWP. However, these additional renderer classes are created by overrides in the `ShellRenderer` class. Therefore, customizing the behavior of these additional renderer classes can be achieved by subclassing them, and creating an instance of the subclass in the appropriate override in the subclassed `ShellRenderer` class.

### iOS example

The following code example shows a subclassed `ShellRenderer`, for iOS, that sets a background image on the navigation bar of the Shell application:

```csharp
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.iOS.MyShellRenderer))]
namespace Xaminals.iOS
{
    public class MyShellRenderer : ShellRenderer
    {
        protected override IShellSectionRenderer CreateShellSectionRenderer(ShellSection shellSection)
        {
            var renderer = base.CreateShellSectionRenderer(shellSection);
            if (renderer != null)
            {
                (renderer as ShellSectionRenderer).NavigationBar.SetBackgroundImage(UIImage.FromFile("monkey.png"), UIBarMetrics.Default);
            }
            return renderer;
        }
    }
}
```

The `MyShellRenderer` class overrides the `CreateShellSectionRenderer` method, and retrieves the renderer created by the base class. It then modifies the renderer by setting a background image on the navigation bar, before returning the renderer.

### Android example

The following code example shows a subclassed `ShellRenderer`, for Android, that sets a background image on the navigation bar of the Shell application:

```csharp
using Android.Content;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

[assembly: ExportRenderer(typeof(Xaminals.AppShell), typeof(Xaminals.Droid.MyShellRenderer))]
namespace Xaminals.Droid
{
    public class MyShellRenderer : ShellRenderer
    {
        public MyShellRenderer(Context context) : base(context)
        {
        }

        protected override IShellToolbarAppearanceTracker CreateToolbarAppearanceTracker()
        {
            return new MyShellToolbarAppearanceTracker(this);
        }
    }
}
```

The `MyShellRenderer` class overrides the `CreateToolbarAppearanceTracker` method, and returns an instance of the `MyShellToolbarAppearanceTracker` class. The `MyShellToolbarAppearanceTracker` class, which derives from the `ShellToolbarAppearanceTracker` class, is shown in the following example:

```csharp
using AndroidX.AppCompat.Widget;
using Xamarin.Forms;
using Xamarin.Forms.Platform.Android;

namespace Xaminals.Droid
{
    public class MyShellToolbarAppearanceTracker : ShellToolbarAppearanceTracker
    {
        public MyShellToolbarAppearanceTracker(IShellContext context) : base(context)
        {
        }

        public override void SetAppearance(Toolbar toolbar, IShellToolbarTracker toolbarTracker, ShellAppearance appearance)
        {
            base.SetAppearance(toolbar, toolbarTracker, appearance);
            toolbar.SetBackgroundResource(Resource.Drawable.monkey);
        }
    }
}
```

The `MyShellToolbarAppearanceTracker` class overrides the `SetAppearance` method, and modifies the toolbar by setting a background image on it.

> [!IMPORTANT]
> It's only necessary to add the `ExportRendererAttribute` to a custom renderer that derives from the `ShellRenderer` class. Additional subclassed Shell renderer classes are created by the subclassed `ShellRenderer` class.

## Related links

- [Xamarin.Forms Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
