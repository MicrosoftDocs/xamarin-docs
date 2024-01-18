---
title: "XAML Hot Reload for Xamarin.Forms"
description: "Reload changes to your XAML file instantly on your running application so you don't have to build your Xamarin.Forms project after every XAML change."
ms.service: xamarin
ms.assetid: E220F054-32EE-424C-A7E5-6156BE271519
ms.subservice: xamarin-forms
author: maddymontaquila
ms.author: maleger
ms.date: 03/01/2021
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# XAML Hot Reload for Xamarin.Forms

XAML Hot Reload plugs into your existing workflow to increase your productivity and save you time. Without XAML Hot Reload, you have to build and deploy your app every time you want to see a XAML change. With Hot Reload, when you save your XAML file the changes are reflected live in your running app. In addition, your navigation state and data will be maintained, enabling you to quickly iterate on your UI without losing your place in the app. Therefore, with XAML Hot Reload, you'll spend less time rebuilding and deploying your apps to validate UI changes.

> [!NOTE]
> If you're writing a native UWP or WPF app, not using Xamarin.Forms, see [XAML Hot Reload for UWP and WPF](/visualstudio/debugger/xaml-hot-reload).

## System requirements

| IDE/Framework | Minimum Version Required |
|------|------------------|
|Visual Studio 2019 | 16.9 for changes only mode, 16.4 for full page mode
Visual Studio 2019 for Mac | 8.9 for changes only mode, 8.4 for full page mode
Xamarin.Forms | 5.0.0.2012 for changes only mode; 4.1 for full page mode

## Enable XAML Hot Reload for Xamarin.Forms

If you are starting from a template, XAML Hot Reload is on by default and the project is configured to work with no additional setup. Debug your Android, iOS, or UWP app on an emulator or physical device and change your XAML to trigger a XAML Hot Reload.

If you're working from an existing Xamarin.Forms solution, no additional installation is required to use XAML Hot Reload, but you might have to double check your configuration to ensure the best experience. First, enable it in your IDE settings:

* On Windows, check the **Enable XAML Hot Reload** checkbox (and the required platforms) at **Tools** > **Options** > **Debugging** > **Hot Reload**.
  * In earlier versions of Visual Studio 2019, the checkbox is at  **Tools** > **Options** > **Xamarin** > **Hot Reload**.
* On Mac, check the **Enable Xamarin Hot Reload** checkbox at **Visual Studio** > **Preferences** > **Tools for Xamarin** > **XAML Hot Reload**.
  * In earlier versions of Visual Studio for Mac, the checkbox is at **Visual Studio** > **Preferences** > **Projects** > **Xamarin Hot Reload**.

Then, in your Android and iOS build settings, check that the Linker is set to "Don't Link" or "Link None". To use XAML Hot Reload with a physical iOS device, you also have to check **Enable the Mono interpreter** (Visual Studio 16.4 and above) or add **--interpreter** to your **Additional mtouch args** (Visual Studio 16.3 and below).

You can use the following flowchart to check your existing project's setup for use with XAML Hot Reload:

![XAML Hot Reload Setup](hot-reload-images/hotreloadflowchart.png "XAML Hot Reload Setup Flowchart")

<!-- https://aka.ms/xamarin-hot-reload-mode points to this section -->
## Hot Reload modes

XAML Hot Reload can work in two different modes - the newer *changes only mode* and the older *full page mode*.

From Visual Studio 16.9 and Visual Studio for Mac 8.9, the default behavior is for changes only mode to be used for all apps that use Xamarin.Forms 5.0 or newer. For older versions of Xamarin.Forms, full page mode is used. However, you can force use of full page mode for all
apps in the Hot Reload IDE settings (**Tools** > **Options** > **Debugging** > **Hot Reload** on Windows or **Visual Studio** > **Preferences** > **Tools for Xamarin** > **XAML Hot Reload** on Mac).

*Changes only mode* parses the XAML to see exactly what changed when you make an edit, and sends just those changes to the running app. This is the same technology used for WPF and UWP Hot Reload. It preserves UI state, since it doesn't recreate the UI for the full page, just updating changed properties on controls affected by edits. Changes only mode also enables use of the [Live Visual Tree](live-visual-tree.md).

By default, with changes only mode you don't need to save your file to see the changes - updates are applied immediately, as you type.
However, you can change this behavior to update only on file save. This can be accomplished by checking the **Apply XAML Hot Reload on document save** checkbox (currently only available on Windows) in the Hot Reload IDE settings. Only updating on document save can sometimes be useful if you make bigger XAML updates and don't wish them to be displayed until they are complete.

*Full page mode* sends the full XAML file to the running app after you makes edits and save. The running app then reloads the page, recreating its controls - you'll see the UI refresh.

Changes only mode is the future of Hot Reload and we recommend using it whenever possible. It's fast, preserves UI state, and supports [Live Visual Tree](live-visual-tree.md). Full page mode is still provided for apps that haven't yet been updated to Xamarin.Forms 5.0.

> [!NOTE]
> You'll need to restart the debug session when switching modes.

## XAML errors

*Changes only mode*: If you make a change the Hot Reload XAML parser sees as invalid, it will show
the error underlined in the editor and include it in the errors window. These Hot Reload errors have an error code starting with "XHR" (for XAML Hot Reload). If there are any such errors on the page, Hot Reload
won't apply changes, even if made on other parts of the page. Fix all the errors for Hot Reload to start working again for the page.

*Full page mode*: If you make a change that XAML Hot Reload can't reload, it will show
the error underlined in the editor and include it in the errors window. These changes, known as rude edits, include mistyping your XAML or wiring a control to an event handler that doesn't exist. Even with a rude edit, you can continue to reload without restarting the app - make another change elsewhere in the XAML file and hit save. The rude edit won't be reloaded, but your other changes will continue to be applied.

## Reload on multiple platforms at once

XAML Hot Reload supports simultaneous debugging in Visual Studio and Visual Studio for Mac. You can deploy an Android and an iOS target at the same time to see your changes reflected on both platforms at once. To debug on multiple platforms, see:

* **Windows** [How To: Set multiple startup projects](/visualstudio/ide/how-to-set-multiple-startup-projects?view=vs-2019&preserve-view=true)
* **Mac** [Set multiple startup projects](/visualstudio/mac/set-startup-projects?view=vsmac-2019&preserve-view=true)

## Known limitations

* Xamarin.Forms targets beyond Android, iOS, and UWP (for example, macOS) aren't currently supported.
* Use of [XamlCompilation(XamlCompilationOptions.Skip)], disabling XAML compilation, isn't supported and can cause issues with the Live Visual Tree.
* You can't add, remove, or rename files or NuGet packages during a XAML Hot Reload session. If you add or remove a file or NuGet package, rebuild and redeploy your app to continue using XAML Hot Reload.
* Set your linker to **Don't Link** or **Link None** for the best experience. The **Link SDK only** setting works most of the time, but it may fail in certain cases. Linker settings can be found in your Android and iOS build options.
* Debugging on a physical iPhone requires the interpreter to use XAML Hot Reload. To do this, open the project settings, select the iOS Build tab, and ensure **Enable the Mono interpreter** setting is enabled. You may need to change the **Platform** option at the top of the property page to **iPhone**.
* XAML Hot Reload can't reload C# code, including event handlers, custom controls, page code-behind, and additional classes.


## Troubleshooting

* Bring up the XAML Hot Reload output to see status messages, which can help in troubleshooting:
  * **Windows**: bring up Output with **View** > **Output** and select **Xamarin Hot Reload** under **Show output from:** at the top   
  * **Mac**: hover over **XAML Hot Reload** in the status bar to show that pad
* If XAML Hot Reload fails to initialize:
  * Update your Xamarin.Forms version.
  * Ensure you are on the latest version of the IDE.
  * Set your Android or iOS Linker settings to **Don't Link** in the project's build settings.
* If nothing happens upon saving your XAML file, ensure that XAML Hot Reload is enabled in the IDE.
* If you're debugging on a physical iPhone and your app becomes unresponsive, check that the interpreter is enabled. To turn it on, check **Enable the Mono interpreter** (Visual Studio 16.4/8.4 and up) or add **--interpreter** to the **Additional mtouch arguments** field (Visual Studio 16.3/8.3 and prior) in your iOS Build settings.

To report a bug, use **Help** > **Send Feedback** > **Report a Problem** on Windows, and **Help** > **Report a Problem** on Mac.

## Related links

- [Live Visual Tree](live-visual-tree.md)
- [Tips and Tricks for XAML Hot Reload](https://devblogs.microsoft.com/xamarin/tips-tricks-xaml-hot-reload/)
- [XAML Hot Reload for Xamarin.Forms In-Depth: The Xamarin Show](https://www.youtube.com/watch?v=crhjjPjzknk)
