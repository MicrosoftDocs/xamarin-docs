---
title: "How can I copy IPA output files to the TFS drop folder?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
---

# How can I copy IPA output files to the TFS drop folder?

Open the `.csproj` file for the iOS app project in a text editor and then add the following lines at the end (immediately before the closing `</Project>` tag):

```xml
<PropertyGroup>
    <CreateIpaDependsOn>
        $(CreateIpaDependsOn);
        CopyIpa
    </CreateIpaDependsOn>
</PropertyGroup>

<Target Name="CopyIpa"
    Condition="'$(OutputType)' == 'Exe'
        And '$(ComputedPlatform)' == 'iPhone'
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## Notes

- This is the same general technique discussed on [Can I change the output path of the IPA file?](~/ios/troubleshooting/questions/ipa-output-path.md). The two important points are to set `$(TF_BUILD_BINARIESDIRECTORY)` as the destination folder and to add an extra condition so `CopyIpa` will only run for TFS builds.

- For a description of `TF_BUILD_BINARIESDIRECTORY` see [Predefined build variables](/azure/devops/pipelines/build/variables).

## Additional references

- [Documentation on installing TFS for use with Xamarin](/azure/devops/repos/tfvc/overview)
- [Azure DevOps Build Task: Xamarin.Android](/azure/devops/pipelines/tasks/build/xamarin-android)
- [Azure DevOps Build Task: Xamarin.iOS](/azure/devops/pipelines/tasks/build/xamarin-ios)

### Next Steps

This document discusses the current behavior as of Xamarin 3.11.666 for Visual Studio and Xamarin.iOS 8.10.3 on the Mac build host. For further assistance, to contact us, or if this issue remains even after utilizing the above information, please see [What support options are available for Xamarin?](~/cross-platform/troubleshooting/support-options.md) for information on contact options, suggestions, as well as how to file a new bug if needed.