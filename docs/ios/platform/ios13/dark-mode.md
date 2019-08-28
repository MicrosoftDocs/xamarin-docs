Dark Mode is a system-wide option for light and dark themes. iOS users may now choose the theme or allow iOS to dynamically change appearance based on the environment and time of day. Xamarin.iOS and Xamarin.Forms deliver native iOS experiences to keep your applications looking shiny and fresh no matter what time of day.
<h2>Turning on Dark Mode</h2>
Before you test that your work to support appearance modes actually works, you’ll need to know how to put your simulator into dark mode. In your chosen iOS 13 simulator open Settings and scroll down to Developer. There you’ll find a switch for turning on or off dark appearance. It takes just a second to see the change take effect across the entire simulator environment. Now you are ready to customize your app for both modes and see your UI respond instantly.

<img class="alignleft size-full wp-image-45184" src="http://devblogs.microsoft.com/xamarin/wp-content/uploads/sites/44/2019/08/LightAndDark_DeveloperSetting.png" alt="Turning on Dark Mode" width="782" height="700" />
<h2>Assets for Light and Dark Modes</h2>
The iOS Asset Catalog in Visual Studio now supports supplying optional images and colors for each appearance mode. When using this functionality, iOS will automatically choose the appropriate image and color for you. Open your Assets.xcassets file in your iOS project and add a new image set. Notice you can specify universal, dark, and light images at any of the target resolutions. In the example here, there is an image for dark and for light with the name of “MicrosoftLogo”.

<img class="alignleft size-full wp-image-45194" src="http://devblogs.microsoft.com/xamarin/wp-content/uploads/sites/44/2019/08/LightAndDark_AssetCatalog2.png" alt="Assets for Light and Dark Modes img1" width="2624" height="1640" />

Notice there are also colors specified for "BackgroundColor" and "TitleColor". Those colors are now available by name to be used throughout the application. The "BackgroundColor" has been assigned to the background of the view, and the "TitleColor" to the label you see on screen.

<img class="alignleft size-full wp-image-45185" src="http://devblogs.microsoft.com/xamarin/wp-content/uploads/sites/44/2019/08/LightAndDark_01.png" alt="Assets for Light and Dark Modes img2" width="782" height="700" />
<h2>Xamarin.Forms</h2>
All of the above applies to Xamarin.Forms as well, but we know many of you prefer to share as much code as possible, and that includes styles. In my sample app <a href="https://github.com/davidortinau/Xappy/" target="_blank" rel="noopener noreferrer">Xappy</a> I have created <a href="https://docs.microsoft.com/en-us/xamarin/xamarin-forms/xaml/resource-dictionaries">resource dictionaries</a> for dark and light themes. Each theme builds upon the base of shared styles for the application.
<pre class="lang:xhtml decode:true" title="DarkTheme">&lt;ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Xappy.Styles.DarkTheme"
             Source="DefaultTheme.xaml"&gt;

    &lt;Color x:Key="backgroundColor"&gt;#FF000000&lt;/Color&gt;
    
    &lt;Color x:Key="TextPrimaryColor"&gt;#B0FFFFFF&lt;/Color&gt;
    &lt;Color x:Key="TextSecondaryColor"&gt;#B0FFFFFF&lt;/Color&gt;
    &lt;Color x:Key="TextTernaryColor"&gt;#C8C8C8&lt;/Color&gt;
&lt;/ResourceDictionary&gt;</pre>
<pre class="lang:xhtml decode:true" title="LightTheme">&lt;ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Xappy.Styles.WhiteTheme"
             Source="DefaultTheme.xaml"&gt;

    &lt;Color x:Key="backgroundColor"&gt;#FFFFFFFF&lt;/Color&gt;

    &lt;Color x:Key="TextPrimaryColor"&gt;#B0000000&lt;/Color&gt;
    &lt;Color x:Key="TextSecondaryColor"&gt;#80000000&lt;/Color&gt;
    &lt;Color x:Key="TextTernaryColor"&gt;#C8C8C8&lt;/Color&gt;
&lt;/ResourceDictionary&gt;</pre>
To make sure the colors update when the styles are changed, you want to use the <a href="https://docs.microsoft.com/en-us/xamarin/xamarin-forms/user-interface/styles/xaml/dynamic"><span class="lang:default decode:true crayon-inline">DynamicResource</span></a>  markup extension. If a style doesn't need to update and change, use the <span class="lang:default decode:true crayon-inline ">StaticResource</span>  markup extension.
<pre class="lang:c# decode:true">Shell.BackgroundColor="{DynamicResource backgroundColor}"
BackgroundColor="{DynamicResource backgroundColor}"
Shell.TitleColor="{StaticResource cerulean}"
</pre>
Since we are using cross-platform styling here, we need to handle when iOS changes the appearance mode. This is easily done in a page renderer by listening to the <a href="https://developer.apple.com/documentation/uikit/uitraitcollection">UITraitCollection</a> change. This is the <a href="https://github.com/davidortinau/Xappy/blob/feature/dark13/Xappy/Xappy.iOS/Renderers/PageRenderer.cs">PageRenderer</a> added to the Xappy.iOS project:
<pre class="lang:c# decode:true">using System;
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;
using Xappy.Styles;

[assembly: ExportRenderer(typeof(ContentPage), typeof(Xappy.iOS.Renderers.PageRenderer))]
namespace Xappy.iOS.Renderers
{
    public class PageRenderer : Xamarin.Forms.Platform.iOS.PageRenderer
    {
        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetAppTheme();
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"\t\t\tERROR: {ex.Message}");
            }
        }

        public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
        {
            base.TraitCollectionDidChange(previousTraitCollection);
            Console.WriteLine($"TraitCollectionDidChange: {TraitCollection.UserInterfaceStyle} != {previousTraitCollection.UserInterfaceStyle}");

            if(this.TraitCollection.UserInterfaceStyle != previousTraitCollection.UserInterfaceStyle)
            {
                SetAppTheme();
            }

            
        }

        void SetAppTheme()
        {
            if (this.TraitCollection.UserInterfaceStyle == UIUserInterfaceStyle.Dark)
            {
                if (App.AppTheme == "dark")
                    return;
                
                App.Current.Resources = new DarkTheme();

                App.AppTheme = "dark";
            }
            else
            {
                if (App.AppTheme != "dark")
                    return;
                App.Current.Resources = new WhiteTheme();
                App.AppTheme = "light";
            }
        }
    }
}
</pre>
This code applies the current theme if it has changed when the view is displayed. This makes sure the app starts in the right mode. Then any time iOS notifies the app that traits have changed, such as <a href="https://developer.apple.com/documentation/uikit/uiuserinterfacestyle">UIUserInterfaceStyle</a>, the code checks that the change is different than the styles currently being displayed as tracked by AppTheme, a simple variable for tracking this.

<img class="alignleft size-full wp-image-45183" src="http://devblogs.microsoft.com/xamarin/wp-content/uploads/sites/44/2019/08/LightAndDark_Forms.png" alt="Xamarin.Forms Dark Mode" width="782" height="700" />
