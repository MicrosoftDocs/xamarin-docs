---
title: "How can I view what libraries are supported in a PCL?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 14FF03BD-AF41-4DB1-B307-2349C13DE7E4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
---

# How can I view what libraries are supported in a PCL?

- You can find an overview of the various features supported by the various PCL target platforms under the *Supported Features* portion of this page: [http://msdn.microsoft.com/en-us/library/gg597391.aspx](https://msdn.microsoft.com/en-us/library/gg597391.aspx)

- Another option is to use the [.NET Portability Analyzer](https://visualstudiogallery.msdn.microsoft.com/1177943e-cfb7-4822-a8a6-e56c7905292b) to assess whether your existing library can be converted to a PCL profile.

- A third possibility is to browse the contents of the actual Profile that you might use. Using  Profile 78 for example, you might go here: 
    `C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\` And view all the assemblies within it.

Whichever method you chose, please note that some functionality has to be downloaded via NuGet and the Microsoft BCL library.
