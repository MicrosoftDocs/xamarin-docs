---
title: "Updating component references to NuGet"
description: "This document describes how to replace your component references with NuGet packages to future-proof your apps, since the Xamarin Component Store has been discontinued."
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9E6C986F-3FBA-4599-8367-FB0C565C0ADE
author: davidortinau
ms.author: daortin
ms.date: 04/18/2018
---

# Updating component references to NuGet

> [!IMPORTANT]
> The Component Store has been discontinued as of May 15, 2018 (this closure was
> originally [announced](https://blog.xamarin.com/hello-nuget-new-home-xamarin-components/) in November 2017).
>
> Xamarin Components are no longer supported in Visual Studio, and should be replaced by
> NuGet packages. Follow the instructions below to manually remove component references
> from your projects.

Refer to these instructions for adding NuGet packages
on [Windows](/nuget/quickstart/use-a-package)
or [Mac](/visualstudio/mac/nuget-walkthrough).

A list of popular Xamarin [plugins and libraries](https://github.com/xamarin/XamarinComponents/blob/master/README.md) is available to help find alternatives to components which are unavailable as NuGet packages.

## Manually removing component references

The 15.6 release of Visual Studio and 7.4 release of Visual Studio for Mac no longer support components in your project.

# [Visual Studio](#tab/windows)

If you load a project into Visual Studio, the following dialog is displayed, explaining that you must remove any components from your project manually:

![Alert dialog explaining that a component has been found in your project and must be removed](component-nuget-images/component-alert-vs.png)

To remove a component from your project:

1. Open the **.csproj** file. To do this, right-click on the project name and select **Unload Project**.

2. Right-click again on the unloaded project and select **Edit {your-project-name}.csproj**.

3. Find any references in the file to `XamarinComponentReference`. It should look similar to the following example:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

4. Remove the references to `XamarinComponentReference` and save the file. In the example above, it's safe to remove the entire `ItemGroup`.

5. Once the file has been saved, right-click on the project name and select **Reload Project**.

6. Repeat the steps above for each project in your solution.

# [Visual Studio for Mac](#tab/macos)

If you load a project into Visual Studio for Mac, the following dialog is displayed, explaining that you must remove any components from your project manually:

![Alert dialog explaining that a component has been found in your project and must be removed](component-nuget-images/component-alert.png)

To remove a component from your project:

1. Open the .csproj file. To do this, right-click on the project name and select **Tools > Edit File**.

2. Find any references in the file to `XamarinComponentReference`. It should look similar to the following example:

    ```xml
    <ItemGroup>
      <XamarinComponentReference Include="advancedcolorpicker">
        <Version>2.0.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="gunmetaltheme">
        <Version>1.4.1</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
      <XamarinComponentReference Include="signature-pad">
        <Version>2.2.0</Version>
        <Visible>False</Visible>
      </XamarinComponentReference>
    </ItemGroup>
    ```

3. Remove the references to `XamarinComponentReference` and save the file. In the example above, it's safe to remove the entire `ItemGroup`

4. Repeat the steps above for each project in your solution.

-----

> [!WARNING]
> The following instructions only work with older versions of Visual Studio.
> The **Components** node is no longer available in the current releases of Visual Studio 2017
> or Visual Studio for Mac.

The following sections explain how to update existing Xamarin solutions
to change component references to NuGet packages.

- [Components that contain NuGet packages](#contain)
- [Components with NuGet replacements](#replace)

Most components fall into one of the above categories.
If you are using a component that does not appear to have an
equivalent NuGet package, read the
[components without a NuGet migration path](#require-update) section below.

<a name="contain"></a>

## Components that contain NuGet packages

Many components already contain NuGet packages, and the migration path
is simply to delete the component reference.

You can determine whether the component already includes a NuGet package
by double-clicking on the component in the solution:

![Components node expanded](component-nuget-images/solution-sml.png)

The **Packages** tab will list any NuGet packages included in the component:

![Packages tab contains NuGet](component-nuget-images/packages-tab-sml.png)

Note that the **Assemblies** tab will be empty:

![Assemblies tab is empty](component-nuget-images/assemblies-tab-empty-sml.png)

### Updating the Solution

To update your solution, delete the **Component** entry from the solution:

![Delete component](component-nuget-images/delete-component-sml.png)

The NuGet package will remain listed in the **Packages** node and your
app will compile and run as usual. In future, updates to this package
will be performed via the **NuGet** update feature:

![Update NuGet package](component-nuget-images/nuget-update-sml.png)

<a name="replace"></a>

## Components with NuGet replacements

If the component info page **Assemblies** tab contains entries as shown below,
you will need to find the equivalent NuGet package manually.

![Contains assemblies](component-nuget-images/assemblies-tab-sml.png)

Note that the **Packages** tab will probably be empty:

![Packages tab](component-nuget-images/packages-tab-empty-sml.png)

_It may contain NuGet dependencies, but these can be ignored._

To confirm a replacement NuGet package exists, search on [NuGet.org](https://www.nuget.org/packages),
using the component name, or alternatively by author.

As an example, you can find the popular **sqlite-net-pcl** package by
searching for:

- [`sqlite-net-pcl`](https://www.nuget.org/packages?q=sqlite-net-pcl) – the product name.
- [`praeclarum`](https://www.nuget.org/packages?q=praeclarum) – the author's profile.

### Updating the Solution

Once you have confirmed the component is available in NuGet, follow these steps:

#### Delete the component

Right click on the component in the solution and choose **Remove**:

![Remove component](component-nuget-images/remove-component-sml.png)

This will delete the component and any references. This will break your build,
until you add the equivalent NuGet package to replace it.

#### Add the NuGet package

1. Right-click on the **Packages** node and choose **Add Packages...**.
2. Search for the NuGet replacement by name or author:

    ![NuGet search](component-nuget-images/nuget-search-sml.png)

3. Press **Add Package**.

The NuGet package will be added to your project, along with any dependencies.
This should fix the build. If the build continues to fail, investigate each
error to see if there were API differences between the component and the NuGet package.

<a name="require-update"></a>

## Components without a NuGet migration path

Don't be concerned if you don't immediately find a replacement for
components used in your application. Existing components will continue to work
in Visual Studio 15.5, and the **Components** node will appear in your solution
as usual.

Future Visual Studio releases, however, will _not_ restore or update components.
This means if you open the solution on a new computer, the component will
not be downloaded and installed; and the author will not be able to provide you
with updates. You should plan to:

- Extract the assemblies from the component and reference them directly in your project.
- Contact the component author and ask about plans to migrate to NuGet.
- Investigate alternative NuGet packages, or seek the source code if the component is open-source.

Many component vendors are still working on migrating to NuGet, and others
(including commercially available products) may be investigating
alternative delivery options.

## Related Links

- [List of popular Xamarin Plugins and Libraries](https://github.com/xamarin/XamarinComponents/blob/master/README.md)
- [Install and use a NuGet package (Windows)](/nuget/quickstart/use-a-package)
- [Including a NuGet package (Mac)](/visualstudio/mac/nuget-walkthrough)