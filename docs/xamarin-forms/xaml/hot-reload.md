---
title: "XAML Hot Reload for Xamarin.Forms"
description: "Reload changes to your XAML file instantly on your running application, so you don't have to build your Xamarin.Forms project after every XAML change."
ms.prod: xamarin
ms.assetid: E220F054-32EE-424C-A7E5-6156BE271519
ms.technology: xamarin-forms
author: maddyleger1
ms.author: maleger
ms.date: 08/13/2019
---

# XAML Hot Reload for Xamarin.Forms (Preview)

XAML Hot Reload plugs into your existing workflow to increase your productivity and save you time. Without XAML Hot Reload, you have to build and deploy your app every time you want to see a XAML change. With Hot Reload, when you save your XAML file the changes are reflected live in your running app. In addition, your navigation state and data will be maintained, enabling you to quickly iterate on your UI without losing your place in the app. Therefore, with XAML Hot Reload, you'll spend less time rebuilding and deploying your apps to validate UI changes.

> [!NOTE]
> If you're writing a WPF or UWP app, see [XAML Hot Reload for UWP and WPF](/visualstudio/debugger/xaml-hot-reload).

## System requirements

| IDE/Framework | Version Required |
|------|------------------|
|Visual Studio 2019 | 16.4 or greater
Visual Studio 2019 for Mac | 8.4 or greater
Xamarin.Forms | 4.1 or greater

## Use XAML Hot Reload for Xamarin.Forms

No additional installation or setup is required to use XAML Hot Reload. It's built into Visual Studio and can be enabled in the IDE settings. Once enabled, you can start using XAML Hot Reload by debugging your app on an emulator, simulator, or physical device. Currently, XAML Hot Reload only works when debugging on iOS or Android.

On Windows, XAML Hot Reload can be enabled by checking the **Enable Xamarin Hot Reload** checkbox at **Tools** > **Options** > **Xamarin** > **Hot Reload**.

On a Mac, XAML Hot Reload can be enabled by checking the **Enable Xamarin Hot Reload** checkbox at **Visual Studio** > **Preferences** > **Projects** > **Xamarin Hot Reload**.

## Resilient reloading

If you make a change that XAML Hot Reload can't reload, it will show you an error message using IntelliSense. These changes, known as rude edits, include mistyping your XAML or wiring a control to an event handler that doesn't exist. Even with a rude edit, you can continue to reload without restarting the app - make another change elsewhere in the XAML file and hit save. The rude edit won't be reloaded, but your other changes will continue to be applied.

## Known limitations

- You can't add, remove, or rename files or NuGet packages during a XAML Hot Reload session. If you add or remove a file or NuGet package, rebuild and redeploy your app to continue using XAML Hot Reload.
- Set the linker to **Don't Link** for the best experience. The **Link SDK only** setting works most of the time, but it may fail in certain cases.
- Debugging on a physical iPhone requires the interpreter to use XAML Hot Reload. To do this, open the project settings, select the iOS Build tab, and ensure **Enable the Mono interpreter** setting is enabled. You may need to change the **Platform** option at the top of the property page to **iPhone**.
- Any references created by assigning a control to another field or property using its `x:Name` value won't be reloaded.
- Updating the visual hierarchy of your Shell application in **AppShell.xaml** can cause issues maintaining the state of your application. Rebuild the app to continue reloading.
- XAML Hot Reload can't reload C# code, including event handlers, custom controls, page code-behind, and additional classes.

## Migrate from the private preview

If you were part of the private preview, your XAML Hot Reload extension will be automatically updated when Visual Studio updates. If you choose not to update Visual Studio, you can continue using the current version of XAML Hot Reload, but you won't receive any further updates through the private preview extension feed.

## Troubleshooting

- If XAML Hot Reload fails to initialize:
  - Update your Xamarin.Forms version.
  - Ensure you are on the latest version of the IDE.
  - Set your Android or iOS Linker settings to **Don't Link** in the project's build settings.
- If nothing happens upon saving your XAML file, ensure that Hot Reload is enabled in the IDE.
- If you're debugging on a physical iPhone and your app becomes unresponsive, check that the interpreter is enabled. To turn it on, open the project settings, select the iOS Build tab, and check the **Enable the Mono interpreter** setting.

To report a bug, use the feedback tool at the **Help** > **Send Feedback** > **Report a Problem** menu on Windows, and the **Help** > **Report a Problem** menu on a Mac.
