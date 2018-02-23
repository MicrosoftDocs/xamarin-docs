---
title: "Verify Attributes"
ms.topic: article
ms.prod: xamarin
ms.assetid: 107FBCEA-266B-4295-B7AA-40A881B82B7B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
---

# Verify Attributes


You will often find that bindings produced by Objective Sharpie will be annotated with the `[Verify]` attribute. These attributes indicate that you should _verify_ that Objective Sharpie did the correct thing by comparing the binding with the original C/Objective-C declaration (which will be provided in a comment above the bound declaration).

Verification is recommended for _all_ bound declarations, but is most likely _required_ for declarations annotated with the `[Verify]` attribute. This is because in many situations, there is not enough metadata in the original native source code to infer how to best produce a binding. You may need to reference documentation or code comments inside the header files to make the best binding decision.

Once you have verified that the binding is correct or have fixed it to be correct, _remove_ the `[Verify]` attribute from the binding.

> [!IMPORTANT]
> `[Verify]` attributes intentionally cause C# compilation errors
so that you are forced to verify the binding. You should remove the
`[Verify]` attribute when you have reviewed (and possibly
corrected) the code.

## Verify Hints Reference

The hint argument supplied to the attribute can be cross referenced with documentation below. Documentation for any produced `[Verify]` attributes will be provided on the console as well after the binding has completed.

<table>
  <thead>
  <tr>
    <th>Verify Hint</th>
    <th>Description</th>
  </tr>
  </thead>
  <tbody>
  <tr>
    <td>InferredFromPreceedingTypedef</td>
    <td>The name of this declaration was inferred by common convention from the immediately preceeding <code>typedef</code> in the original native source code. Verify that the inferred name is correct as this convention is ambiguous.</td>
  </tr>
  <tr>
    <td>ConstantsInterfaceAssociation</td>
    <td>There's no fool-proof way to determine with which Objective-C interface an extern variable declaration may be associated. Instances of these are bound as <code>[Field]</code> properties in a partial interface into a near-by concrete interface to produce a more intuitive API, possibly eliminating the 'Constants' interface altogether.</td>
  </tr>
  <tr>
    <td>MethodToProperty</td>
    <td>An Objective-C method was bound as a C# property due to convention such as taking no parameters and returning a value (non-void return). Often methods like these should be bound as properties to surface a nicer API, but sometimes false-positives can occur and the binding should actually be a method.</td>
  </tr>
  <tr>
    <td>StronglyTypedNSArray</td>
    <td>A native <code>NSArray*</code> was bound as <code>NSObject[]</code>. It might be possible to more strongly type the array in the binding based on expectations set through API documentation (e.g. comments in the header file) or by examining the array contents through testing. For example, an NSArray* containing only NSNumber* instancescan be bound as <code>NSNumber[]</code> instead of <code>NSObject[]</code>.</td>
  </tr>
  </tbody>
</table>

You can also quickly receive documentation for a hint using the `sharpie verify-docs` tool, for example:

```csharp
sharpie verify-docs InferredFromPreceedingTypedef
```

