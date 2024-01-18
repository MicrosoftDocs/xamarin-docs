---
title: "Custom Linker Configuration"
description: "This document describes an XML file that can be used to configure the linker, ensuring explicitly that needed code is not eliminated from the linked application."
ms.service: xamarin
ms.assetid: F8A99E3F-2197-4399-AC81-F1DBAB5729C9
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
no-loc: [Objective-C]
---

# Custom Linker Configuration

If the default set of options is not enough, you can drive the linking
process with an XML file that describes what you want from the linker.

You can provide extra definitions to the linker to ensure the type, methods
and/or fields are not eliminated from your application. In your own code the
preferred way is to use the `[Preserve]` custom attribute, as discussed
in the [Linking on iOS](~/ios/deploy-test/linker.md) and
[Linking on Android](~/android/deploy-test/linker.md)
guides.
However if you need some definitions from the SDK or product assemblies then using an
XML file might be your best solution (versus adding code that will ensure the
linker won't eliminate what you need).

To do this, you define an XML file with the top-level element
`<linker>` which contains *assembly* nodes which in turn contain *type* nodes, which in turn contain *method* and *field*
nodes.

Once you have this linker description file, add it to your project and:

- **For Android** : set the  **Build Action** to **LinkDescription**
- **For iOS** : set the  **Build Action** to **LinkDescription**

The following example shows what the XML file looks like:

```xml
<linker>
        <assembly fullname="mscorlib">
                <type fullname="System.Environment">
                        <field name="mono_corlib_version" />
                        <method name="get_StackTrace" />
                </type>
        </assembly>
        <assembly fullname="My.Own.Assembly">
                <type fullname="Foo" preserve="fields">
                        <method name=".ctor" />
                </type>
                <type fullname="Bar">
                        <method signature="System.Void .ctor(System.String)" />
                        <field signature="System.String _blah" />
                </type>
                <namespace fullname="My.Own.Namespace" />
                <type fullname="My.Other*" />
        </assembly>
</linker>
```

In the above example, the linker will read and apply the instructions on the `mscorlib.dll` (shipped with Mono for Android) and `My.Own.Assembly` (user code) assemblies.

The first section, for `mscorlib.dll`, will ensure that the `System.Environment` type will preserve its field named `mono_corlib_version` and its `get_StackTrace` method.
Note the getter and/or setter method names must be used as the linker works on
IL and does not understand C# properties.

The second section, for `My.Own.Assembly.dll`, will ensure that
the `Foo` type will preserve all its fields (i.e. the `preserve="fields"` attribute) and all its constructors (i.e. all
the methods named `.ctor` in IL). The `Bar` type
will preserve specific signatures (not names) for one constructor (that
accepts a single string parameter) and for a specific string field `_blah`.
The `My.Own.Namespace` namespace will preserve all the types it contains.
Lastly, any type whose full name (including the namespace) matches the wildcard
pattern "My.Other\*" will preserve all of its fields and methods. The wildcard
character `*` can be included multiple times within a "type fullname" pattern.

## Related Links

- [Linking on iOS](~/ios/deploy-test/linker.md)
- [Linking on Android](~/android/deploy-test/linker.md)
- [Linker GitHub repo with examples](https://github.com/mono/linker)
