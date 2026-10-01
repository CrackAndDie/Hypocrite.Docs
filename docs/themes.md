---
sidebar_position: 3
id: themes
title: Theme management
---

# Theme management   

**Hypocrite.Services** supports realtime theme change and flexible object registrations. Here is the sample code to register ```ThemeSwitcherService``` in your App (if you don't want the theme change service in your project you can skip this registration):

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>  

    <TabItem value="wpf" label="WPF" default>    

        ```csharp
        containerRegistry.RegisterInstance(new ThemeSwitcherService<ThemeType>()
        {
            NameOfDictionary = "ThemeHolder",
            ThemeSources = new Dictionary<ThemeType, string>()
            {
                { ThemeType.Dark, "/Hypocrite.DemoWpf;component/Resources/Themes/DarkTheme.xaml" },
                { ThemeType.Light, "/Hypocrite.DemoWpf;component/Resources/Themes/LightTheme.xaml" },
            },
        });
        ```  

    </TabItem>  

    <TabItem value="avalonia" label="Avalonia">  

        ```csharp
        containerRegistry.RegisterInstance(new ThemeSwitcherService<ThemeType>(ThemeType.Dark)
        {
            NameOfDictionary = "ThemeHolder",
            ThemeSources = new Dictionary<ThemeType, string>()
            {
                { ThemeType.Dark, "avares://Hypocrite.DemoAvalonia/Resources/Themes/DarkTheme.axaml" },
                { ThemeType.Light, "avares://Hypocrite.DemoAvalonia/Resources/Themes/LightTheme.axaml" },
            },
        });
        ```  

    </TabItem>  

</Tabs>   

The ```ThemeSwitcherService``` get the startup/default theme of your project. About the other parameters we will talk a bit later.  

In my app ```ThemeType``` is an enum of themes for the app:
```csharp
public enum ThemeType
{
    Dark,
    Light
}
```

### Ceating resource dictionaries

For proper work of *ThemeSwitcherService* You should create *ResourceDictionaries* for each theme You have and the *ResourceDictionary* that will hold all the changes of themes. So in my project I've created *DarkTheme.xaml*, *LightTheme.xaml* and *ThemeHolder.xaml*.  

<Tabs
    defaultValue="wpf"
    values={[
        {label: 'WPF', value: 'wpf'},
        {label: 'Avalonia', value: 'avalonia'},
    ]}>
    <TabItem value="wpf">  
       *DarkTheme.xaml*:
        ```xml
        <ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
            <Color x:Key="TextForegroundBrushColor">AliceBlue</Color>
            <SolidColorBrush x:Key="TextForegroundBrush" 
                            Color="{DynamicResource TextForegroundBrushColor}"/>

            <Color x:Key="WindowBrushColor">#070c13</Color>
            <SolidColorBrush x:Key="WindowBrush"
                            Color="{DynamicResource WindowBrushColor}" />
        </ResourceDictionary>
        ```
        *LightTheme.xaml*:
        ```xml
        <ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
            <Color x:Key="TextForegroundBrushColor">Black</Color>
            <SolidColorBrush x:Key="TextForegroundBrush"
                            Color="{DynamicResource TextForegroundBrushColor}" />
            
            <Color x:Key="WindowBrushColor">#fefefe</Color>
            <SolidColorBrush x:Key="WindowBrush"
                            Color="{DynamicResource WindowBrushColor}" />
        </ResourceDictionary>
        ```
        *ThemeHolder.xaml* (You should set here a default theme):
        ```xml
        <ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="/Hypocrite.DemoWpf;component/Resources/Themes/DarkTheme.xaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
        ```

        The holder of *.xaml* theme files (*ThemeHolder.xaml* in our example) should be merged into Your app resources like this:
        ```xml
        <mvvm:ApplicationBase x:Class="YourNamespace.App"
                            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                            xmlns:local="clr-namespace:YourNamespace"
                            xmlns:mvvm="clr-namespace:Hypocrite.Mvvm;assembly=Hypocrite.Wpf">
            <Application.Resources>
                <ResourceDictionary>
                    <ResourceDictionary.MergedDictionaries>
                        <ResourceDictionary Source="/Hypocrite.DemoWpf;component/Resources/Themes/ThemeHolder.xaml"/>
                    </ResourceDictionary.MergedDictionaries>
                </ResourceDictionary>
            </Application.Resources>
        </mvvm:ApplicationBase>
        ```
    </TabItem>
    <TabItem value="avalonia">
        *DarkTheme.xaml*:
        ```xml
        <ResourceDictionary xmlns="https://github.com/avaloniaui"
                            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
            <Color x:Key="TextForegroundBrushColor">AliceBlue</Color>
            <SolidColorBrush x:Key="TextForegroundBrush" 
                            Color="{DynamicResource TextForegroundBrushColor}"/>

            <Color x:Key="WindowBrushColor">#070c13</Color>
            <SolidColorBrush x:Key="WindowBrush"
                            Color="{DynamicResource WindowBrushColor}" />
        </ResourceDictionary>
        ```
        *LightTheme.xaml*:
        ```xml
        <ResourceDictionary xmlns="https://github.com/avaloniaui"
                            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
            <Color x:Key="TextForegroundBrushColor">Black</Color>
            <SolidColorBrush x:Key="TextForegroundBrush"
                            Color="{DynamicResource TextForegroundBrushColor}" />
            
            <Color x:Key="WindowBrushColor">#fefefe</Color>
            <SolidColorBrush x:Key="WindowBrush"
                            Color="{DynamicResource WindowBrushColor}" />
        </ResourceDictionary>
        ```
        *ThemeHolder.xaml* (You should set here a default theme):
        ```xml
        <ResourceDictionary xmlns="https://github.com/avaloniaui"
                            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
            <ResourceDictionary.MergedDictionaries>
                    <ResourceInclude Source="/Resources/Themes/DarkTheme.axaml"/>
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
        ```

        The holder of *.xaml* theme files (*ThemeHolder.xaml* in our example) should be merged into Your app resources like this:
        ```xml
        <Application xmlns="https://github.com/avaloniaui"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                    x:Class="YourNamespace.App">
            <Application.Resources>
                <ResourceDictionary>
                    <ResourceDictionary.MergedDictionaries>
                        <ResourceInclude Source="/Resources/Themes/ThemeHolder.axaml"/>
                    </ResourceDictionary.MergedDictionaries>
                </ResourceDictionary>
            </Application.Resources>
        </Application>
        ```
    </TabItem>
</Tabs>  

### Usage in *xaml*

Registered colors and brushes could be used as *DynamicResource*s like that:
```xml
<Rectangle Fill="{DynamicResource TestBrush}"/>
```

### Changing and handling current theme

Using the ```ThemeSwitcherService``` you can change an App's theme in realtime using method ```ChangeTheme(theme)``` and get current theme using method ```GetCurrentTheme()```.   

There is also a ```ThemeChangedEvent``` event that is called through ```IEventAggregator``` with ```ThemeChangedEventArgs```. You can subscribe like this (```IEventAggregator``` is already resolved in ```ViewModelBase```):
```csharp
EventAggregator.GetEvent<ThemeChangedEvent>().Subscribe(YourHandler);
```
