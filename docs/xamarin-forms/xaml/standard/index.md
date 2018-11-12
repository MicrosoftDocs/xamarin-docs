---
title: "XAML Standard (Preview)"
description: "This article explains how to get started with exploring the XAML Standard Preview in Xamarin.Forms."
ms.prod: xamarin
ms.assetid: 24382DF1-BE70-4608-B86F-B79FB23E4A78
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/15/2017
---

# XAML Standard (Preview)

![Preview](~/media/shared/preview.png)

Follow these steps to experiment with XAML Standard in Xamarin.Forms:

# [Visual Studio](#tab/windows)

1. Download the [preview NuGet package here](https://aka.ms/xf-xamlstandard-nuget).
2. Add the **Xamarin.Forms.Alias** NuGet package to your Xamarin.Forms .NET Standard and platform projects.
3. Initialize the package with `Alias.Init()`
4. Add an `xmlns:a` reference `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Use the types in XAML - see the [Controls reference](controls.md) for more information.

# [Visual Studio for Mac](#tab/macos)

1. Download the [preview NuGet package here](https://aka.ms/xf-xamlstandard-nuget).
2. Add the **Xamarin.Forms.Alias** NuGet package to your Xamarin.Forms .NET Standard and platform projects.
3. Initialize the package with `Alias.Init()`
4. Add an `xmlns:a` reference `xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"`
5. Use the types in XAML - see the [Controls reference](controls.md) for more information.

-----

The following XAML demonstrates some XAML Standard controls being used
in a Xamarin.Forms `ContentPage`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ContentPage 
    xmlns="http://xamarin.com/schemas/2014/forms" 
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" 
    xmlns:a="clr-namespace:Xamarin.Forms.Alias;assembly=Xamarin.Forms.Alias"
    x:Class="XAMLStandardSample.ItemsPage" 
    Title="{Binding Title}" x:Name="BrowseItemsPage">
    <ContentPage.ToolbarItems>
        <ToolbarItem Text="Add" Clicked="AddItem_Clicked" />
    </ContentPage.ToolbarItems>
    <ContentPage.Content>
        <a:StackPanel>
            <ListView x:Name="ItemsListView" ItemsSource="{Binding Items}" VerticalOptions="FillAndExpand" HasUnevenRows="true" RefreshCommand="{Binding LoadItemsCommand}" IsPullToRefreshEnabled="true" IsRefreshing="{Binding IsBusy, Mode=OneWay}" CachingStrategy="RecycleElement" ItemSelected="OnItemSelected">
                <ListView.ItemTemplate>
                    <DataTemplate>
                        <ViewCell>
                            <StackLayout Padding="10">
                                <a:TextBlock Text="{Binding Text}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemTextStyle}" FontSize="16" />
                                <a:TextBlock Text="{Binding Description}" LineBreakMode="NoWrap" Style="{DynamicResource ListItemDetailTextStyle}" FontSize="13" />
                            </StackLayout>
                        </ViewCell>
                    </DataTemplate>
                </ListView.ItemTemplate>
            </ListView>
        </a:StackPanel>
    </ContentPage.Content>
</ContentPage>
```

> [!NOTE]
> Requiring the xmlns `a:` prefix on the XAML Standard controls is a limitation of the current preview.


## Related Links

- [Preview NuGet](https://aka.ms/xf-xamlstandard-nuget)
- [Controls Reference](controls.md)
