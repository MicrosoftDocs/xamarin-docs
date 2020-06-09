---
title: ".xib Code Generation in Xamarin.iOS"
description: "This document describes how Xamarin.iOS generates code to map .xib files to C#, making visual controls accessible programmatically."
ms.prod: xamarin
ms.assetid: 365991A8-E07A-0420-D28E-BC4D32065E1A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
---

# .xib Code Generation in Xamarin.iOS

> [!IMPORTANT]
> This document explains Visual Studio for Mac's integration with Xcode's Interface Builder only,
> as Actions and Outlets are not used in the Xamarin Designer for iOS. For more information on the iOS Designer,
> please review the [iOS Designer](~/ios/user-interface/designer/index.md) document.

The Apple Interface Builder tool ("IB") can be used to design user
interfaces visually. The interface definitions created by IB are saved in **.xib** files. Widgets and other objects in **.xib** files may be given a "class identity",
which can be a custom user-defined type. This allows you to customize the
behavior of widgets and to write custom widgets.

These user classes are normally subclasses of UI controller classes. They
have *outlets* (analogous to properties) and *actions* (analogous
to events) that can be connected to interface objects. At runtime, when the IB
file is loaded, the objects are created, and the outlets and actions are
connected to the various UI objects dynamically. When defining these managed
classes, you must define all the actions and outlets to match the ones that IB
expects. Visual Studio for Mac uses a CodeBehind-like model to simplify this. This is
similar to what Xcode does for Objective-C, but the code generation model and
conventions have been tweaked to be more familiar to .NET developers.

Working with **.xib** files is not currently supported in Xamarin.iOS for Visual Studio.

## .xib Files and Custom Classes

As well as using existing types from Cocoa Touch, it is possible to define
custom types in **.xib** files. It is also possible to use types that are defined in
other **.xib** files, or defined purely in C# code. Currently, Interface Builder is not aware of the details of types defined outside the current **.xib** file, so it will not list them or show their custom outlets and actions. Removing this limitation is planned for sometime in the future.

Custom classes can be defined in a **.xib** file using the "Add Subclass" command
in the "Classes" tab of Interface Builder. We refer to these as "CodeBehind"
classes. If the **.xib** file has a ".xib.designer.cs" counterpart file in the
project, then Visual Studio for Mac will automatically fill it with partial classes
definitions for all the custom classes in the **.xib**. We refer to these partial
classes as "designer classes".

## Generating Code

For any **{0}.xib** file with a build action of *Page*, if a **{0}.xib.designer.cs** file also exists in the
 project, Visual Studio for Mac will
generate partial classes in the designer file for all of the user classes it can
find in the **.xib** file, with properties for the outlets and partial methods for
all of the actions. Code generation is enabled simply by the presence of this
file.

The designer file is automatically updated when the **.xib** file changes and
Visual Studio for Mac regains focus. The designer file should not be modified manually, as
changes will be overwritten next time Visual Studio for Mac updates the file.

## Registration and Namespaces

Visual Studio for Mac generates the designer classes using the project's default
namespace for the designer file location, to make it consistent with normal .NET
project namespacing. The namespace of designer files is driven by the project's
"default namespace" and its ".NET naming policies" settings. Beware that if your
project's default namespace changes, MD will regenerate the classes in the new
namespace, so you may find that your partial classes no longer match up.

To make the class discoverable by the Objective-C runtime, Visual Studio for Mac
applies a `[Register (name)]` attribute to the class. Although Xamarin.iOS automatically registers `NSObject`-derived classes, it uses the fully-qualified .NET names. The attribute applied by Visual Studio for Mac overrides this to ensure each class is registered with the name used in the **.xib** file. If you use custom
classes in IB without using Visual Studio for Mac to generate designer files, you may have
to apply this manually to make your managed classes match up to the expected
Objective-C class names.

Classes cannot be defined in more than one **.xib**, or they will conflict.

## Non-Designer Class Parts

Designer partial classes are not intended to be used as-is. Outlets are
private, and no base class is specified. It is expected that each class will have a corresponding "non-designer" class part in another file, which sets the base class, uses or exposes the outlets, and defines constructors that are required to instantiate the class from native code when loading the **.xib**. The default **.xib** templates do this, but for any additional custom classes you define in a **.xib**, you must add the non-designer part manually.

The reason for this is the need for flexibility. For example, multiple CodeBehind classes could subclass a common managed abstract class, which subclasses the class to be subclassed by IB.

It is conventional to put these in a **{0}.xib.cs** file beside the **{0}.xib.designer.cs** designer file.

<a name="generated"></a>

## Generated Actions and Outlets

In the partial designer classes, Visual Studio for Mac generates properties
corresponding to any connected outlets defined in IB, and partial methods
corresponding to any connected actions.

### Outlet Properties

Designer classes contain properties corresponding to all outlets defined on
the custom class. The fact that these are properties is an implementation detail
of the Xamarin.iOS to Objective C bridge, to enable lazy binding. You should
consider them to be equivalent to private fields, intended to be used only from the
CodeBehind class. If you wish to make them public, add accessor properties to
the non-designer class part, as you would for any other private field.

If outlet properties are defined to have a type of `id` (equivalent to
`NSObject`) then the designer code generator currently determines the strongest
possible type based on objects connected to that outlet, for convenience.
However, this may not be supported in future versions, so it is recommended that
you explicitly strongly type the outlets when defining the custom class.

### Action Properties

Designer classes contain partial methods corresponding to all actions defined
on the custom class. These are methods without an implementation. The purpose of
the partial methods is twofold:

1. If you type  `partial` in the class body of the non-designer class part, Visual Studio for Mac will
offer to autocomplete the signatures of all non-implemented partial methods.
2. The partial method signatures have an attribute applied that exposes them to the Objective-C world,
so they can get handled as the corresponding action.

If you wish, you may ignore the partial method, and implement the action by
applying the attribute to a different method, or let it fall through to a base
class.

If actions are defined to have a sender type of `id` (equivalent to `NSObject`),
then the designer code generator currently determines the strongest possible
type based on objects connected to that action. However, this
may not be supported in future versions, so it is recommended that you
explicitly strongly type the actions when defining the custom class.

Note that these partial methods are created only for C#, because CodeDOM
doesn't support partial methods, so they are not generated for other
languages.

## Cross-XIB Class Usage

Sometimes, users wish to reference the same class from multiple **.xib** files, for
example with tab controllers. This can be done by explicitly referencing the
class definition from another **.xib** file, or by defining the same class name again
in the second **.xib**.

The latter case can be problematic due to Visual Studio for Mac processing **.xib** files individually. It cannot automatically detect and merge duplicate definitions, so you may end up with conflicts applying the Register attribute multiple times when the same partial class is defined in multiple designer files. Recent versions of Visual Studio for Mac attempt to resolve this, but it may not
always work as expected. In future this is likely to become unsupported, and
instead Visual Studio for Mac will make all types defined in all **.xib** files and managed
code in the project directly visible from all **.xib** files.

## Type Resolution

Types used in IB are Objective-C type names. These are mapped to CLR types
through the use of Register attributes. When generating outlet and action code,
Visual Studio for Mac will resolve the corresponding CLR types for all Objective-C types
wrapped by the Xamarin.iOS core, and fully qualify their type names.

However, the code generator cannot currently resolve CLR types from
Objective-C type names in user code or libraries, so in such cases it outputs
the type name verbatim. This means that the corresponding CLR type must have the
same name as the Objective-C type and be in the same namespace as the code
that's using it. This is planned to be fixed sometime in the future by
considering all Objective-C types in the project during code generation.
