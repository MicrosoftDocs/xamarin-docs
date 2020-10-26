---
title: "Can I change the output path of the IPA file?"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F5E5DCC6-F7CC-48E2-89E8-709E9C269502
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
---

# Can I change the output path of the IPA file?

## For Cycle 7 and higher
Yes, you can use customized MSBuild targets to achieve this. The easiest option is probably to copy the `.ipa` file after it has been built.

These steps will work for any iOS project that uses the MSBuild build engine on either Mac or Windows. (Note: all Unified API projects use the MSBuild build engine.)

1. Open the `.csproj` file for the iOS app project in a text editor and then add the following lines at the end (immediately before the closing `</Project>` tag):

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
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(IpaPackagePath)"
            DestinationFolder="$(OutputPath)"
        />
    </Target>
    ```

2. Set the DestinationFolder to the desired output folder. As usual you may use MSBuild properties (like $(OutputPath)) within this argument if you wish.

## Notes

- The `CreateIpaDependsOn` property is defined in the `Xamarin.iOS.Common.targets` file that is part of Xamarin.iOS. It behaves as described in the [Overriding Predefined Targets](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) section of the article [How to: Extend the Visual Studio Build Process](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process).

- You could use a **Move** Task rather than a **Copy** Task if you preferred. If you choose that option and you are building on Windows, you will need to use the fully-qualified task name `<Microsoft.Build.Tasks.Move>` to avoid an ambiguity with the XamarinVS build tasks.

## For versions before Xamarin Studio 6.0.0.5174 | Xamarin for Visual Studio 4.1.0.530

Yes, you can use customized MSBuild targets to achieve this. The easiest option is probably to copy the `.ipa` file after it has been built.

These steps will work for any iOS project that uses the MSBuild build engine on either Mac or Windows. (Note: all Unified API projects use the MSBuild build engine.)

1. Open the `.csproj` file for the iOS app project in a text editor and then add the following lines at the end (immediately before the closing `</Project>` tag).

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
            And '$(BuildIpa)' == 'true'">
        <Copy
            SourceFiles="$(OutputPath)$(IpaPackageName)"
            DestinationFolder="/Users/macuser/Desktop/"
        />
    </Target>
    ```

2. Set the `DestinationFolder` to the desired output folder. As usual you may use MSBuild properties (like `$(OutputPath)`) within this argument if you wish.

## Notes

- The `CreateIpaDependsOn` property is defined in the `Xamarin.iOS.Common.targets` file that is part of Xamarin.iOS. t behaves as described in the [Overriding Predefined Targets](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process#overriding-predefined-targets) section of the article [How to: Extend the Visual Studio Build Process](/visualstudio/msbuild/how-to-extend-the-visual-studio-build-process).

- You could use a **Move** Task rather than a **Copy** Task if you preferred. If you choose that option and you are building on Windows, you will need to use the fully-qualified task name `<Microsoft.Build.Tasks.Move>` to avoid an ambiguity with the XamarinVS build tasks.