---
title: "Advanced (manual) Real-World Example"
description: "This document describes how to use the output of xcodebuild as the input to Objective Sharpie, which provides insight into what Objective Sharpie does under the hood."
ms.prod: xamarin
ms.assetid: 044FF669-0B81-4186-97A5-148C8B56EE9C
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
---

# Advanced (manual) Real-World Example

**This example uses the [POP library from Facebook](https://github.com/facebook/pop).**

This section covers a more advanced approach to binding, where we will use Apple's `xcodebuild` tool to first build the POP project, and then manually deduce input for Objective Sharpie. This essentially covers what Objective Sharpie is doing under the hood in the previous section.

```
 $ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   _(more git clone output)_

$ cd pop
```

Because the POP library has an Xcode project (`pop.xcodeproj`), we can just use `xcodebuild` to build POP. This process may in turn generate header files that Objective Sharpie may need to parse. This is why building before binding is important. When building via `xcodebuild` ensure you pass the same SDK identifier and architecture that you intend to pass to Objective Sharpie (and remember, Objective Sharpie 3.0 can usually do this for you!):

```
$ xcodebuild -sdk iphoneos9.0 -arch arm64

Build settings from command line:
    ARCHS = arm64
    SDKROOT = iphoneos8.1
 
=== BUILD TARGET pop OF PROJECT pop WITH THE DEFAULT CONFIGURATION (Release) ===
 
...
 
CpHeader pop/POPAnimationTracer.h build/Headers/POP/POPAnimationTracer.h
    cd /Users/aaron/src/sharpie/pop
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/Users/aaron/bin::/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin:/usr/local/git/bin:/Users/aaron/.rvm/bin"
    builtin-copy -exclude .DS_Store -exclude CVS -exclude .svn -exclude .git -exclude .hg -strip-debug-symbols -strip-tool /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/strip -resolve-src-symlinks /Users/aaron/src/sharpie/pop/pop/POPAnimationTracer.h /Users/aaron/src/sharpie/pop/build/Headers/POP
 
...
 
** BUILD SUCCEEDED **
```

There will be a lot of build information output in the console as part of `xcodebuild`. As displayed above, we can see that a "CpHeader" target was run wherein header files were copied to a build output directory. This is often the case, and makes binding easier: as part of the native library's build, header files are often copied into a "publicly" consumable location which can make parsing easier for binding. In this case, we know that POP's header files are in the `build/Headers` directory.

We are now ready to bind POP. We know that we want to build for SDK `iphoneos8.1` with the `arm64` architecture, and that the header files we care about are in `build/Headers` under the POP git checkout. If we look in the `build/Headers` directory, we'll see a number of header files:

```
$ ls build/Headers/POP/
POP.h                    POPAnimationTracer.h     POPDefines.h
POPAnimatableProperty.h  POPAnimator.h            POPGeometry.h
POPAnimation.h           POPAnimatorPrivate.h     POPLayerExtras.h
POPAnimationEvent.h      POPBasicAnimation.h      POPPropertyAnimation.h
POPAnimationExtras.h     POPCustomAnimation.h     POPSpringAnimation.h
POPAnimationPrivate.h    POPDecayAnimation.h
```

If we look at `POP.h`, we can see it is the library's main top-level header file that `#import`s other files. Because of this, we only need to pass `POP.h` to Objective Sharpie, and clang will do the rest behind the scenes:

```
$ sharpie bind -output Binding -sdk iphoneos8.1 \
    -scope build/Headers build/Headers/POP/POP.h \
    -c -Ibuild/Headers -arch arm64

Parsing Native Code...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Binding Analysis:
  Automated binding is complete, but there are a few APIs which have
  been flagged with [Verify] attributes. While the entire binding
  should be audited for best API design practices, look more closely
  at APIs with the following Verify attribute hints:

  ConstantsInterfaceAssociation (1 instance):
    There's no fool-proof way to determine with which Objective-C
    interface an extern variable declaration may be associated.
    Instances of these are bound as [Field] properties in a partial
    interface into a near-by concrete interface to produce a more
    intuitive API, possibly eliminating the 'Constants' interface
    altogether.

  StronglyTypedNSArray (4 instances):
    A native NSArray* was bound as NSObject[]. It might be possible
    to more strongly type the array in the binding based on
    expectations set through API documentation (e.g. comments in the
    header file) or by examining the array contents through testing.
    For example, an NSArray* containing only NSNumber* instances can
    be bound as NSNumber[] instead of NSObject[].

  MethodToProperty (2 instances):
    An Objective-C method was bound as a C# property due to
    convention such as taking no parameters and returning a value (
    non-void return). Often methods like these should be bound as
    properties to surface a nicer API, but sometimes false-positives
    can occur and the binding should actually be a method.

  Once you have verified a Verify attribute, you should remove it
  from the binding source code. The presence of Verify attributes
  intentionally cause build failures.

  For more information about the Verify attribute hints above,
  consult the Objective Sharpie documentation by running 'sharpie
  docs' or visiting the following URL:

    http://xmn.io/sharpie-docs

Submitting usage data to Xamarin...
  Submitted - thank you for helping to improve Objective Sharpie!

Done.
```

You will notice that we passed a `-scope build/Headers` argument to Objective Sharpie. Because C and Objective-C libraries must `#import` or `#include` other header files that are implementation details of the library and not API you wish to bind, the `-scope` argument tells Objective Sharpie to ignore any API that is not defined in a file somewhere within the `-scope` directory.

You will find the `-scope` argument is often optional for cleanly implemented libraries, however there is no harm in explicitly providing it.

Additionally, we specified `-c -Ibuild/headers`. Firstly, the `-c` argument tells Objective Sharpie to stop interpreting command line arguments and pass any subsequent arguments _directly to the clang compiler_. Therefore, `-Ibuild/Headers` is a clang compiler argument that instructs clang to search for includes under `build/Headers`, which is where the POP headers live. Without this argument, clang would not know where to locate the files that `POP.h` is `#import`ing. _Almost all "issues" with using Objective Sharpie boil down to figuring out what to pass to clang_.

### Completing the Binding

Objective Sharpie has now generated `Binding/ApiDefinitions.cs` and `Binding/StructsAndEnums.cs` files.

These are Objective Sharpie's basic first pass at the binding, and in a few
cases it might be all you need. As stated above however, the developer will
usually need to manually modify the generated files after Objective Sharpie
finishes to
[fix any issues](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md)
that could not be automatically handled by the tool.

Once the updates are complete, these two files can now be added to a binding
project in Visual Studio for Mac or be passed directly to the `btouch` or `bmac`
tools to produce the final binding.

For a thorough description of the binding process, please see our
[Complete Walkthrough instructions](~/ios/platform/binding-objective-c/walkthrough.md).
