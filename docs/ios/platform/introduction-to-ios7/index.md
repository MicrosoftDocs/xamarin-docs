---
title: "Introduction to iOS 7"
description: "This article covers the major new APIs introduced in iOS 7, including View Controller transitions, enhancements to UIView animations, UIKit Dynamics and Text Kit. It also covers some of the changes to the user interface, and the new enchanced multitasking capabilities."
ms.service: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
no-loc: [Objective-C]
---

# Introduction to iOS 7

_This article covers the major new APIs introduced in iOS 7, including View Controller transitions, enhancements to UIView animations, UIKit Dynamics and Text Kit. It also covers some of the changes to the user interface, and the new enchanced multitasking capabilities._

iOS 7 is a major update to iOS. It introduces a completely new user interface design that puts focus on content rather than application chrome. Alongside the visual changes, iOS 7 adds a plethora of new APIs to create richer interactions and experiences. This document surveys the new technologies introduced with iOS 7, and serves as a starting point for further exploration.

## UIView Animation Enhancements

iOS 7 augments the animation support in UIKit, allowing applications to do things that previously required dropping directly into the Core Animation framework. For example, `UIView` can now perform spring animations as well as keyframe animations, which previously a `CAKeyframeAnimation` applied to a `CALayer`.

### Spring Animations

 `UIView` now supports animating property changes with a spring effect. To add this, call either the `AnimateNotify` or `AnimateNotifyAsync` method, passing in values for the spring damping ratio and the initial spring velocity, as described below:

- `springWithDampingRatio` – A value between 0 and 1, where the oscillation increases for smaller value.
- `initialSpringVelocity` – The initial spring velocity as a percentage of the total animation distance per second.

The following code produces a spring effect when the image view’s center changes:

```csharp
void AnimateWithSpring ()
{
    float springDampingRatio = 0.25f;
    float initialSpringVelocity = 1.0f;

    UIView.AnimateNotify (3.0, 0.0, springDampingRatio, initialSpringVelocity, 0, () => {

        imageView.Center = new CGPoint (imageView.Center.X, 400);

    }, null);
}
```

This spring effect causes the image view to appear to bounce as it completes its animation to a new center location, as illustrated below:

 ![This spring effect causes the image view to appear to bounce as it completes its animation to a new center location](images/spring-animation.png)

### Keyframe Animations

The `UIView` class now includes the `AnimateWithKeyframes` method for creating keyframe animations on a `UIView`. This method is similar to other `UIView` animation methods, except that an additional `NSAction` is passed as a parameter to include the keyframes. Within the `NSAction`, keyframes are added by calling `UIView.AddKeyframeWithRelativeStartTime`.

For example, the following code snippet creates a keyframe animation to animate a view’s center as well as to rotate the view:

```csharp
void AnimateViewWithKeyframes ()
{
    var initialTransform = imageView.Transform;
    var initialCeneter = imageView.Center;

    // can now use keyframes directly on UIView without needing to drop directly into Core Animation

    UIView.AnimateKeyframes (2.0, 0, UIViewKeyframeAnimationOptions.Autoreverse, () => {
        UIView.AddKeyframeWithRelativeStartTime (0.0, 0.5, () => {
            imageView.Center = new CGPoint (200, 200);
        });

        UIView.AddKeyframeWithRelativeStartTime (0.5, 0.5, () => {
            imageView.Transform = CGAffineTransform.MakeRotation ((float)Math.PI / 2);
        });
    }, (finished) => {
        imageView.Center = initialCeneter;
        imageView.Transform = initialTransform;

        AnimateWithSpring ();
    });
}
```

The first two parameters to the `AddKeyframeWithRelativeStartTime` method specify the start time and duration of the keyframe, respectively, as a percentage of the overall animation length. The example above results in the image view animating to its new center over the first second, followed by rotating 90 degrees over the next second. Since the animation specifies `UIViewKeyframeAnimationOptions.Autoreverse` as an option, both keyframes animate in reverse as well. Finally, the final values are set to the initial state in the completion handler.

The screenshots below illustrates the combined animation through the keyframes:

 ![This screenshots illustrates the combined animation through the keyframes](images/keyframes.png)

## UIKit Dynamics

UIKit Dynamics is a new set of APIs in UIKit that allow applications to create animated interactions based on physics. UIKit Dynamics encapsulates a 2D physics engine to make this possible.

The API is declarative in nature. You declare how the physics interactions behave by creating objects - called *behaviors* - to express physics concepts such as gravity, collisions, springs, etc. Then you attach the behavior(s) to another object, called a *dynamic animator*, which encapsulates a view. The dynamic animator takes cares of applying the declared physics behaviors to *dynamic items* - items that implement `IUIDynamicItem`, such as a `UIView`.

There are several different primitive behaviors available to trigger complex interactions, including:

- `UIAttachmentBehavior` – Attaches two dynamic items such that they move together, or attaches a dynamic item to an attachment point.
- `UICollisionBehavior` – Allows dynamic items to participate in collisions.
- `UIDynamicItemBehavior` – Specifies a general set of properties to apply to dynamic items, such as elasticity, density and friction.
- `UIGravityBehavior` - Applies gravity to a dynamic item, causing items to accelerate in the gravitational direction.
- `UIPushBehavior` – Applies force to a dynamic item.
- `UISnapBehavior` – Allows a dynamic item to snap to a position with a spring effect.

Although there are many primitives, the general process for adding physics-based interactions to a view using UIKit Dynamics is consistent across behaviors:

1. Create a dynamic animator.
1. Create behavior(s).
1. Add behaviors to the dynamic animator.

### Dynamics Example

Let’s look at an example that adds gravity and a collision boundary to a `UIView`.

#### UIGravityBehavior

Adding gravity to an image view follows the 3 steps outlined above.

We’ll work in the `ViewDidLoad` method for this example. First, add a `UIImageView` instance as follows:

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

This creates an image view centered at the top edge of the screen. To make the image “fall” with gravity, create an instance of a `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

The `UIDynamicAnimator` takes an instance of a reference `UIView` or a `UICollectionViewLayout`, which contains the items that will be animated per the attached behavior(s).

Next, create a `UIGravityBehavior` instance. You can
pass one or more objects implementing the `IUIDynamicItem`, like a `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

The behavior is passed an array of `IUIDynamicItem`, which in this case contains the single `UIImageView` instance we are animating.

Finally, add the behavior to the dynamic animator:

```csharp
dynAnimator.AddBehavior (gravity);
```

This results in the image animating downward with gravity, as illustrated below:

![The starting image location](images/gravity2.png)
![The ending image location](images/gravity3.png)

Since there is nothing constraining the boundaries of the screen, the image view simply falls off the bottom. To constrain the view so that the image collides with the edges of the screen, we can add a `UICollisionBehavior`. We'll cover this in the next section.

#### UICollisionBehavior

We'll begin by creating a `UICollisionBehavior` and adding it to the dynamic animator, just like we did for the `UIGravityBehavior`.

Modify the code to include the `UICollisionBehavior`:

```csharp
using (image = UIImage.FromFile ("monkeys.jpg")) {

    imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
        Image =  image
    };

    View.AddSubview (imageView);

    // 1. create the dynamic animator
    dynAnimator = new UIDynamicAnimator (this.View);

    // 2. create behavior(s)
    var gravity = new UIGravityBehavior (imageView);
    var collision = new UICollisionBehavior (imageView) {
        TranslatesReferenceBoundsIntoBoundary = true
    };

    // 3. add behaviors(s) to the dynamic animator
    dynAnimator.AddBehaviors (gravity, collision);
}
```

The `UICollisionBehavior` has a property called `TranslatesReferenceBoundsIntoBoundry`. Setting this to `true` causes the reference view’s bounds to be used as a collision boundary.

Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there.

<!--, as shown below:

 ![Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there](images/bounce.png)-->

#### UIDynamicItemBehavior

We can further control the behavior of the falling image view with additional behaviors. For example, we could add a `UIDynamicItemBehavior` to increase the elasticity, causing the image view to bounce more when it collides with the bottom of the screen.

Adding a `UIDynamicItemBehavior` follows the same steps as with the other behaviors. First create the behavior:

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

Then, add the behavior to the dynamic animator:

 `dynAnimator.AddBehavior (dynBehavior);`

With this behavior in place, the image view bounces more when it collides with the boundary.

## General User Interface Changes

In addition to the new UIKit APIs such as UIKit Dynamics, Controller transitions, and enhanced UIView animations described above, iOS 7 introduces a variety of visual changes to the UI, and related API changes for various views and controls. For more information see the [iOS 7 User Interface Overview](~/ios/platform/introduction-to-ios7/ios7-ui.md).

## Text Kit

Text Kit is a new API that offers powerful text layout and rendering features. It is built on top of the low level Core Text framework, but is much easier to use than Core Text.

For more information, please see our [TextKit](~/ios/platform/textkit.md)

## Multitasking

iOS 7 changes when and how background work is performed. Task completion in iOS 7 no longer keeps applications awake when tasks are running in the background, and applications are woken for background processing in a non-contiguous manner. iOS 7 also adds three new APIs for updating applications with new content in the background:

- Background Fetch – Allows applications to update content in the background at regular intervals.
- Remote Notifications - Allows applications to update content when receiving a push notification. The notifications can be either silent or can display a banner on the lock screen.
- Background Transfer Service – Allows uploading and downloading of data, such as large files, without a fixed time limit.

For more details about the new multitasking capabilities, see the iOS sections of the Xamarin [Backgrounding guide](~/ios/app-fundamentals/backgrounding/index.md).

## Summary

This article covers several major new additions to iOS. First, it shows how to add custom transitions to View Controllers. Then, it shows how to use transitions in collection views, both from within a navigation controller, as well as interactively between collection views. Next, it introduces several enhancements made to UIView animations, showing how applications use UIKit for things that previously required programming directly against Core Animation. Finally, new UIKit Dynamics API, which brings a physics engine to UIKit, is introduced alongside the rich text support now available in the Text Kit framework.

## Related Links

- [Intro to iOS 7 (sample)](/samples/xamarin/ios-samples/introtoios7)
- [iOS 7 User Interface Overview](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Backgrounding](~/ios/app-fundamentals/backgrounding/index.md)