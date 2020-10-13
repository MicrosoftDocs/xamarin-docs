---
title: "Packaging Wear Apps"
description: "This article explains how to package Android Wear apps."
ms.prod: xamarin
ms.assetid: E32DD855-78DD-46F8-B234-4EAC0756BDA2
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/02/2018
---

# Packaging Wear Apps

> [!WARNING]
> The following docs and sample projects may no longer be maintained.
> As of [Xamarin.Android 11.1][xa-11.1], automatically packaging an
> Android Wear application within an Android handheld application is
> no longer supported. It is recommended to distribute Android Wear
> applications as [standalone applications][standalone] instead.

Android Wear 1.0 apps are packaged with a full Android app for 
distribution on Google Play.

Android Wear 2.0 apps can be submitted to Google Play as [standalone
applications][standalone].

[xa-11.1]: /xamarin/android/release-notes/11/11.1
[standalone]: https://developer.android.com/training/wearables/apps/standalone-apps

## Automatic Packaging

Starting with Xamarin Android 5.0, your Wear app is automatically 
packaged as a resource in your Handheld app when you create a project 
reference from the Handheld project to the Wear project. You can use 
the following steps to create this association: 

# [Visual Studio](#tab/windows)

1. If your Wear app is not already part of your Handheld solution,
   right-click the solution node and select **Add > Add Existing
   Project...**.

2. Navigate to the **.csproj** file of your Wear app, select it, and
   click **Open**. The Wear app project should now be visible in your
   Handheld solution.

3. Right-click the **References** node and select **Add Reference**.

4. In the **Reference Manager** dialog, enable your Wear project (click
   to add a check mark), then click **OK**.

5. Change the package name for your Wear project so that it matches
   the package name of the Handheld project (the package name can be
   changed under **Properties > Android Manifest**).

# [Visual Studio for Mac](#tab/macos)

1. If your Wear app is not already part of your Handheld solution,
   right-click the solution node and select **Add > Add Existing
   Project...**.

2. Navigate to the **.csproj** file of your Wear app, select it, and
   click **Open**. The Wear app project should now be visible in your
   Handheld solution.

3. Right-click the Handheld project node in the solution and click
   **Edit References...**.

4. In the **Edit References** dialog, enable your Wear project (click
   to add a check mark), then click **OK**.

5. Change the package name for your Wear project so that it matches 
   the package name of the Handheld project (the package name can be
   changed under **Project Options > Android Application**).

-----

Note that you will get an **XA5211** error if the package name of the
Wear app does not match the package name of the Handheld app. For
example:

```shell
Error XA5211: Embedded wear app package name differs from handheld 
app package name (com.companyname.mywearapp != com.companyname.myapp). (XA5211)
```

To correct this error, change the package name of the Wear app so
that it matches the package name of the Handheld app.

When you click **Build > Build All**, this association triggers
automatic packaging of the Wear project into the main Handheld (Phone)
project. The Wear app is automatically built and included as a resource
in the Handheld app.

The assembly that the Wear app project generates is not used as an
assembly reference in the Handheld (Phone) project. Instead, the build
process does the following:

- Verifies that the package names match. 

- Generates XML and adds it to the Handheld project to 
    associate it with the Wear app. For example: 

    ```xml
    <!-- Handheld (Phone) Project.csproj -->
    <ProjectReference Include="..\MyWearApp\MyWearApp.csproj">
        <Project>{D80E1FEF-653B-448C-B2AA-609C74E88340}</Project>
        <Name>MyWearApp</Name>
        <IsAppExtension>True</IsAppExtension>
    </ProjectReference>
    ```

- Adds the Wear app as a **raw** resource to the Handheld project. 

## Manual Packaging

You can write Android Wear apps in Xamarin.Android before version 5.0, 
but you must follow these manual packaging instructions to distribute 
the app: 

1. Ensure that your Wearable project and Handheld (Phone) projects
   have the same version number and package name.

2. Manually build the Wearable project as a **Release** build.

3. Manually add the release **.APK** from step (2) into the
   **Resources/raw** directory of the Handheld (Phone) project.

4. Manually add a new XML resource
   **Resources/xml/wearable_app_desc.xml** in the Handheld project
   which refers to Wearable **APK** from step (3):

    ```xml
    <wearableApp package="wearable.app.package.name">
        <versionCode>1</versionCode>
        <versionName>1.0</versionName>
        <rawPathResId>NAME_OF_APK_FROM_STEP_3</rawPathResId>
    </wearableApp>
    ```

5. Manually add a `<meta-data />` element to the Handheld     project's
   **AndroidManifest.xml** `<application>` element     that refers to the
   new XML resource:

    ```xml
    <meta-data android:name="com.google.android.wearable.beta.app"
        android:resource="@xml/wearable_app_desc"/>
    ```

See also the Android Developer site's 
[manual packging instructions](https://developer.android.com/training/wearables/apps/packaging.html#PackageManually).