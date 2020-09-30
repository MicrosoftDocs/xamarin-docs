---
title: "Can I update the Xamarin.Forms default template to a newer NuGet package?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Can I update the Xamarin.Forms default template to a newer NuGet package?

This guide uses the Xamarin.Forms .NET Standard library template as an example, but the same general method will also work for the Xamarin.Forms Shared Project template. This guide is written with the example of updating from Xamarin.Forms 1.5.1.6471 to 2.1.0.6529, but the same steps are possible to set other versions as the default instead.

1. Copy the original template `.zip` from:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2. Unzip the `.zip` to a temporary location.

3. Change all of the occurrences of the old version of the Xamarin.Forms package to the new version you'd like to use.
    * `FormsTemplate\FormsTemplate.vstemplate`
    * `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    * `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Example: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`

4. Change the "name" element of the main [multi-project template file](/visualstudio/ide/how-to-create-multi-project-templates) (`Xamarin.Forms.PCL.vstemplate`) to make it unique. For example:

    > `<Name>Blank App (Xamarin.Forms Portable) - 2.1.0.6529</Name>`

5. Re-zip the whole template folder. Make sure to match the original file structure of the `.zip` file. The `Xamarin.Forms.PCL.vstemplate` file should be at the top of the `.zip` file, not within any folders.

6. Create a "Mobile Apps" subdirectory in your per-user Visual Studio templates folder:
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7. Copy the new zipped-up template folder into the new "Mobile Apps" directory.

8. Download the NuGet package that matches the version from step 3. For example, [https://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529](https://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (see also [https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file)), and copy it into the appropriate subfolder of the Xamarin Visual Studio extensions folder:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`