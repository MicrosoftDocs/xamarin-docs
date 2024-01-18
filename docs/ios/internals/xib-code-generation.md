---
title: ".xib Code Generation in Xamarin.iOS"
description: "This document describes how Xamarin.iOS generates code to map '.xib' files to C#, making visual controls accessible programmatically."
ms.service: xamarin
ms.assetid: 365991A8-E07A-0420-D28E-BC4D32065E1A
ms.subservice: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
no-loc: [Objective-C]
---

# .xib Code Generation in Xamarin.iOS

The Apple Interface Builder tool ("IB") can be used to design user interfaces visually. 
The interface definitions created by IB are saved in **.xib** files. Widgets and other objects in **.xib** files may be given a "class identity",
which can be a custom user-defined type. Using custom types allows you to customize the
behavior of widgets and to write custom widgets.

These user classes are normally subclasses of UI controller classes. 
They have *outlets* (properties) and *actions* (events) that can be connected to interface objects.
At runtime, the IB is loaded. 
At that time, the objects are created, and the outlets and actions are connected to the various UI objects dynamically. 
When defining these managed classes, you must define all the actions and outlets to match the ones that IB expects. 
Visual Studio for Mac uses a CodeBehind-like model to simplify the code. 
Xcode has a similar Objective-C model. But the Xamarin.iOS code generation model and conventions have been tweaked to be more familiar to .NET developers.

## .xib Files and Custom Classes

It's possible to define custom types in **.xib** files, in addition to using the existing types from Cocoa Touch. It's also possible to use types that are defined in
other **.xib** files, or defined purely in C# code. Currently, Interface Builder isn't aware of the details of types defined outside the current **.xib** file, so it won't list them or show their custom outlets and actions. Removing this limitation is planned for sometime in the future.

Custom classes can be defined in a **.xib** file using the "Add Subclass" command
in the "Classes" tab of Interface Builder. We refer to those classes as "CodeBehind"
classes. If the **.xib** file has a ".xib.designer.cs" counterpart file in the
project, then Visual Studio for Mac will automatically fill it with partial classes
definitions for all the custom classes in the **.xib**. We refer to these partial
classes as "designer classes".

## Generating Code

Code generation is enabled by the presence of a **{0}.xib.designer.cs** file, for any **{0}.xib** file with a build action of *Page*.
Visual Studio for Mac generates partial classes in the designer file for all of the user classes it can
find in the **.xib** file. Visual Studio for Mac generates properties for outlets and partial methods for
actions. 

The designer file is automatically updated when the **.xib** file changes and
Visual Studio for Mac regains focus. Making changes to the designer file isn't recommended as
changes will be overwritten next time Visual Studio for Mac updates the file.

## Registration and Namespaces

Visual Studio for Mac generates the designer classes using the project's default namespace for the designer file location.
This behavior is consistent with normal .NET project namespace generation.
The namespace of designer files uses the project's "default namespace" and its ".NET naming policies" settings. 
If your project's default namespace changes, the regenerated classes will use the new namespace.
After regeneration, you may find your partial classes no longer match up.

To make the class discoverable by the Objective-C runtime, Visual Studio for Mac
applies a `[Register (name)]` attribute to the class. Although Xamarin.iOS automatically registers `NSObject`-derived classes, it uses the fully qualified .NET names. The attribute applied by Visual Studio for Mac overrides the Xamarin.iOS behavior to ensure each class is registered with the name used in the **.xib** file. Add the attribute manually for all custom classes defined using IB, without using Visual Studio for Mac to generate designer files. Doing so makes your managed classes match up to the expected Objective-C class names.

Classes cannot be defined in more than one **.xib**, or they'll conflict.

## Non-Designer Class Parts

Designer partial classes aren't intended to be used as-is. Outlets are private, and no base class is specified. It's expected that each class will have a corresponding "non-designer" class part in another file. The "non-designer" file sets the base class, manipulates the outlets, and defines constructors that are required to instantiate the class from native code. The default **.xib** templates have the "non-designer" class parts, but for any other custom classes you define in a **.xib**, you must add the non-designer part manually.

This separation using partial classes is needed for flexibility. For example, multiple CodeBehind classes could subclass a common managed abstract class, which subclasses the class to be subclassed by IB.

It's conventional to put CodeBehind classes in a **{0}.xib.cs** file beside the **{0}.xib.designer.cs** designer file.

<a name="generated"></a>

## Generated Actions and Outlets

In the partial designer classes, Visual Studio for Mac generates properties
corresponding to any connected outlets defined in IB, and partial methods
corresponding to any connected actions.

### Outlet Properties

Designer classes contain properties corresponding to all outlets defined on
the custom class. These properties enable lazy binding. They're an implementation detail
of the Xamarin.iOS to Objective C bridge. Think of them as 
equivalent to private fields, intended to be used only from the
CodeBehind class. Make the field public by adding the public accessor to the field in the non-designer class part.

If outlet properties are defined to have a type of `id` (equivalent to
`NSObject`), then the designer code generator currently determines the strongest
possible type based on objects connected to that outlet, for convenience.
However, this behavior may not be supported in future versions. It's recommended that
you explicitly strongly type the outlets when defining the custom class.

### Action Properties

Designer classes contain partial methods corresponding to all actions defined
on the custom class. These methods don't have an implementation. The purpose of
the partial methods is twofold:

1. If you type  `partial` in the class body of the non-designer class part, Visual Studio for Mac will
offer to autocomplete the signatures of all non-implemented partial methods.
2. The partial method signatures have an attribute applied that exposes them to the Objective-C world,
so they can get handled as the corresponding action.

You may ignore the partial method, and implement the action by
applying the attribute to a different method. Or let it fall through to a base
class.

If actions are defined to have a sender type of `id` (equivalent to `NSObject`),
then the designer code generator currently determines the strongest possible
type based on objects connected to that action. However, this behavior
may not be supported in future versions. It's recommended that you
explicitly strongly type the actions when defining the custom class.

These partial methods are created only for C#, because CodeDOM
doesn't support partial methods. They aren't generated for other
languages.

## Cross-XIB Class Usage

Sometimes, users wish to reference the same class from multiple **.xib** files, for
example with tab controllers. You can explicitly reference the
class definition from another **.xib** file, or define the same class name again
in the second **.xib**.

The latter case can be problematic because of Visual Studio for Mac processing **.xib** files individually. Visual Studio for Mac can't detect and merge duplicate definitions. You may end up with conflicts applying the Register attribute multiple times when the same partial class is defined in multiple designer files. Recent versions of Visual Studio for Mac attempt to resolve the conflicts, but it may not
always work as expected. In the future, this behavior is likely to become unsupported, and instead Visual Studio for Mac will make all types defined in all **.xib** files and managed
code in the project directly visible from all **.xib** files.

## Type Resolution

Types used in IB are Objective-C type names mapped to CLR types by using Register attributes. When generating code, Visual Studio for Mac will resolve CLR types, fully qualifying the type names to the Objective-C types. These Objective-C types are wrapped by the Xamarin.iOS core.

The code generator can't currently resolve CLR types from Objective-C type names in user code or libraries. 
In such cases, it outputs the type name verbatim. It must have the same name as the Objective-C type to properly resolve the CLR type.
The CLR Type must be in the same namespace as the code that's using it. Future versions of the code generator will consider all Objective-C types in the project.
