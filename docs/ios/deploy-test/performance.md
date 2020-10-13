---
title: "Xamarin.iOS performance"
description: "This document describes techniques that can be used to improve performance and memory usage in Xamarin.iOS applications."
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/29/2016
---

# Xamarin.iOS performance

Poor application performance presents itself in many ways. It can make an application seem unresponsive, can cause slow scrolling, and can reduce battery life. However, optimizing performance involves more than just implementing efficient code. The user's experience of application performance must also be considered. For example, ensuring that operations execute without blocking the user from performing other activities can help to improve the user's experience.

This document describes techniques that can be used to improve performance and memory usage in Xamarin.iOS applications.

> [!NOTE]
> Before reading this article you should first read [Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md), which discusses non-platform specific techniques to improve the memory usage and performance of applications built using the Xamarin platform.

## Avoid strong circular references

In some situations it's possible to create strong reference cycles that could prevent objects from having their memory reclaimed by the garbage collector. For example, consider the case where an [`NSObject`](xref:Foundation.NSObject)-derived subclass, such as a class that inherits from [`UIView`](xref:UIKit.UIView), is added to an `NSObject`-derived container and is strongly referenced from Objective-C, as shown in the following code example:

```csharp
class Container : UIView
{
    public void Poke ()
    {
    // Call this method to poke this object
    }
}

class MyView : UIView
{
    Container parent;
    public MyView (Container parent)
    {
        this.parent = parent;
    }

    void PokeParent ()
    {
        parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

When this code creates the `Container` instance, the C# object will have a strong reference to an Objective-C object. Similarly, the `MyView` instance will also have a strong reference to an Objective-C object.

In addition, the call to `container.AddSubview` will increase the reference count on the unmanaged `MyView` instance. When this happens, the Xamarin.iOS runtime creates a `GCHandle` instance to keep the `MyView` object in managed code alive, because there is no guarantee that any managed objects will keep a reference to it. From a managed code perspective, the `MyView` object would be reclaimed after the [`AddSubview`](xref:UIKit.UIView.AddSubview(UIKit.UIView)) call were it not for the `GCHandle`.

The unmanaged `MyView` object will have a `GCHandle` pointing to the managed object, known as a *strong link*. The managed object will contain a reference to the `Container` instance. In turn the `Container` instance will have a managed reference to the `MyView` object.

In circumstances where a contained object keeps a link to its container, there are several options available to deal with the circular reference:

- Manually break the cycle by setting the link to the container to `null`.
- Manually remove the contained object from the container.
- Call `Dispose` on the objects.
- Avoid the circular reference keeping a weak reference to the container. For more information about weak references.

### Using WeakReferences

One way to prevent a cycle is to use a weak reference from the
child to the parent. For example, the above code could be written like
this:

```csharp
class Container : UIView
{
    public void Poke ()
    {
        // Call this method to poke this object
    }
}

class MyView : UIView
{
    WeakReference<Container> weakParent;
    public MyView (Container parent)
    {
        this.weakParent = new WeakReference<Container> (parent);
    }

    void PokeParent ()
    {
        if (weakParent.TryGetTarget (out var parent))
            parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

Here, the contained object will not keep the parent alive. However,
the parent keeps the child alive through the call done to
`container.AddSubView`.

This also happens in iOS APIs that use the delegate or data
source pattern, where a peer class contains the implementation;
for example, when setting the
[`Delegate`](xref:UIKit.UITableView.Delegate*)
property or the
[`DataSource`](xref:UIKit.UITableView.DataSource*)
in the [`UITableView`](xref:UIKit.UITableView) class.

In the case of classes that are created purely for the sake of
implementing a protocol, for example the
[`IUITableViewDataSource`](xref:UIKit.IUITableViewDataSource),
what you can do is instead of creating a subclass, you can just
implement the interface in the class and override the method, and
assign the `DataSource` property to `this`.

#### Weak attribute

[Xamarin.iOS
11.10](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_11/xamarin.ios_11.10.md#WeakAttribute)
introduced the `[Weak]` attribute. Like `WeakReference <T>`,
`[Weak]` can be used to break [strong circular
references](#avoid-strong-circular-references),
but with even less code.

Consider the following code, which uses `WeakReference <T>`:

```csharp
public class MyFooDelegate : FooDelegate {
    WeakReference<MyViewController> controller;
    public MyFooDelegate (MyViewController ctrl) => controller = new WeakReference<MyViewController> (ctrl);
    public void CallDoSomething ()
    {
        MyViewController ctrl;
        if (controller.TryGetTarget (out ctrl)) {
            ctrl.DoSomething ();
        }
    }
}
```

Equivalent code using `[Weak]` is far more concise:

```csharp
public class MyFooDelegate : FooDelegate {
    [Weak] MyViewController controller;
    public MyFooDelegate (MyViewController ctrl) => controller = ctrl;
    public void CallDoSomething () => controller.DoSomething ();
}
```

The following is another example of using `[Weak]` in context of the
[delegation](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html)
pattern:

```csharp
public class MyViewController : UIViewController
{
    WKWebView webView;

    protected MyViewController (IntPtr handle) : base (handle) { }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        webView = new WKWebView (View.Bounds, new WKWebViewConfiguration ());
        webView.UIDelegate = new UIDelegate (this);
        View.AddSubview (webView);
    }
}

public class UIDelegate : WKUIDelegate
{
    [Weak] MyViewController controller;

    public UIDelegate (MyViewController ctrl) => controller = ctrl;

    public override void RunJavaScriptAlertPanel (WKWebView webView, string message, WKFrameInfo frame, Action completionHandler)
    {
        var msg = $"Hello from: {controller.Title}";
        var alertController = UIAlertController.Create (null, msg, UIAlertControllerStyle.Alert);
        alertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
        controller.PresentViewController (alertController, true, null);
        completionHandler ();
    }
}
```

### Disposing of objects with strong references

If a strong reference exists and it's difficult to remove the dependency, make a `Dispose` method clear the parent pointer.

For containers, override the `Dispose` method to remove the contained objects, as shown in the following code example:

```csharp
class MyContainer : UIView
{
    public override void Dispose ()
    {
        // Brute force, remove everything
        foreach (var view in Subviews)
        {
              view.RemoveFromSuperview ();
        }
        base.Dispose ();
    }
}
```

For a child object that keeps strong reference to its parent, clear the reference to the parent in the `Dispose` implementation:

```csharp
class MyChild : UIView
{
    MyContainer container;
    public MyChild (MyContainer container)
    {
        this.container = container;
    }
    public override void Dispose ()
    {
        container = null;
    }
}
```

For more information about releasing strong references, see
[Release IDisposable Resources](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable).
There's also a good discussion in this blog post:
[Xamarin.iOS, the garbage collector and me](https://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me).

### More information

For more information, see [Rules to Avoid Retain Cycles](https://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html) on Cocoa With Love, [Is this a bug in MonoTouch GC](https://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc) on StackOverflow, and [Why can't MonoTouch GC kill managed objects with refcount > 1?](https://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1) on StackOverflow.

## Optimize table views

Users expect smooth scrolling and fast load times for [`UITableView`](xref:UIKit.UITableView) instances. However, scrolling performance can suffer when cells contain deeply nested view hierarchies, or when cells contain complex layouts. However, there are techniques that can be used to avoid poor `UITableView` performance:

- Reuse cells. For more information, see [Reuse Cells](#reuse-cells).
- Reduce the number of subviews.
- Cache cell content that is retrieved from a web service.
- Cache the height of any rows if they aren't identical.
- Make the cell, and any other views, opaque.
- Avoid image scaling and gradients.

Collectively these techniques can help to keep [`UITableView`](xref:UIKit.UITableView) instances scrolling smoothly.

### Reuse cells

When displaying hundreds of rows in a [`UITableView`](xref:UIKit.UITableView), it would be a waste of memory to create hundreds of [`UITableViewCell`](xref:UIKit.UITableViewCell) objects when only a small number of them are displayed on screen at once. Instead, only the cells visible on screen can be loaded into memory, with the **content** being loaded into these reused cells. This prevents the instantiation of hundreds of additional objects, saving time and memory.

Therefore, when a cell disappears from the screen its view can be placed in a queue for reuse, as shown in the following code example:

```csharp
class MyTableSource : UITableViewSource
{
    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        // iOS will create a cell automatically if one isn't available in the reuse pool
        var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

        // Perform required cell actions
        return cell;
    }
}
```

As the user scrolls, the [`UITableView`](xref:UIKit.UITableView) calls the `GetCell` override to request new views to display. This override then calls the [`DequeueReusableCell`](xref:UIKit.UITableView.DequeueReusableCell(Foundation.NSString)) method and if a cell is available for reuse it will be returned.

For more information, see [Cell Reuse](~/ios/user-interface/controls/tables/populating-a-table-with-data.md) in [Populating a Table with Data](~/ios/user-interface/controls/tables/populating-a-table-with-data.md).

## Use opaque views

Ensure that any views that have no transparency defined have their [`Opaque`](xref:UIKit.UIView.Opaque) property set. This will ensure that the views are optimally rendered by the drawing system. This is particularly important when a view is embedded in a [`UIScrollView`](xref:UIKit.UIScrollView), or is part of a complex animation. Otherwise the drawing system will composite the views with other content, which can noticeably impact on performance.

## Avoid fat XIBs

Although XIBs have largely been replaced by storyboards, there are some circumstances where XIBs may still be used. When a XIB is loaded into memory, all of its contents are loaded into memory, including any images. If the XIB contains a view that's not immediately being used, then memory is being wasted. Therefore, when using XIBs ensure that there is only one XIB per view controller, and if possible, separate the view controller's view hierarchy into separate XIBs.

## Optimize image resources

Images are some of the most expensive resources that applications use, and are often captured at high resolutions. Therefore, when displaying an image from the app's bundle in a [`UIImageView`](xref:UIKit.UIImageView), ensure that the image and `UIImageView` are identically sized. Scaling images at runtime can be an expensive operation, particularly if the `UIImageView` is embedded in a [`UIScrollView`](xref:UIKit.UIScrollView).

For more information, see [Optimize Image Resources](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages) in the [Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md) guide.

## Test on devices

Begin deploying and testing an application on a physical device as early as possible. Simulators do not perfectly match the behaviors and limitations of devices and so it is important to test in a real-world device scenario as early as possible.

In particular the simulator does not in any way simulate the memory or CPU restrictions of a physical device.

## Synchronize animations with the display refresh

Games tend to have tight loops to run the game logic and update the screen. Typical frame rates range from thirty to sixty frames per second. Some developers feel that they should update the screen as many times as possible per second, combining their game simulation with updates to the screen and might be tempted to go beyond sixty frames per second.

However, the display server performs screen updates at an upper limit of sixty times per second. Therefore, attempting to update the screen faster than this limit can lead to screen tearing and micro-stuttering. It's best to structure code so that screen updates are synchronized with the display update. This can be achieved by using the [`CoreAnimation.CADisplayLink`](xref:CoreAnimation.CADisplayLink) class, which is a timer suitable for visualization and games that runs at sixty frames per second.

## Avoid Core Animation transparency

Avoiding core animation transparency improve bitmap compositing performance. In general, avoid transparent layers and blurred borders if possible.

## Avoid code generation

Generating code dynamically with `System.Reflection.Emit` or the *Dynamic Language Runtime* must be avoided because the iOS kernel prevents dynamic code execution.

## Summary

This article described and discussed techniques for increasing the performance of applications built with Xamarin.iOS. Collectively these techniques can greatly reduce the amount of work being performed by a CPU, and the amount of memory consumed by an application.

## Related links

- [Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)