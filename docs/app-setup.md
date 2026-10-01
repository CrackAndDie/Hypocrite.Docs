---
sidebar_position: 1
id: app-setup
title: Application setup
---

# Application setup  

Before gettings started with the features of the **Hypocrite.Services** you should do some stuff with your **App** files.  

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>  

<TabItem value="wpf" label="WPF" default>  

        The first step is to add *PackageReference* to your project:  

        ```xml
        <ItemGroup>
            ...
            <PackageReference Include="Hypocrite.Services" Version="SpecifyVersionHere" />
            ...
        </ItemGroup>
        ```

        Then change your **App.xaml** file as follows. Where ```YourNamespace``` is the namespace where **App.xaml** is located.  
        ```xml
        <mvvm:ApplicationBase x:Class="YourNamespace.App"
                              xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                              xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
                              xmlns:local="clr-namespace:YourNamespace"
                              xmlns:mvvm="clr-namespace:Hypocrite.Mvvm;assembly=Hypocrite.Wpf">
        </mvvm:ApplicationBase>
        ```
        Then change your **App.xaml.cs** file so the ```App``` class whould be inherited from ```ApplicationBase```. The assembly of your project should be registered in ```IViewModelResolverService``` so you should override ```CreateShell``` method. 
        ```csharp
        public partial class App : ApplicationBase
        {
            protected override Window CreateShell()
            {
                var viewModelService = Container.Resolve<IViewModelResolverService>();
                viewModelService.RegisterViewModelAssembly(Assembly.GetExecutingAssembly());
                // ...

                return base.CreateShell();
            }

            protected override void RegisterTypes(IContainerRegistry containerRegistry)
            {
                base.RegisterTypes(containerRegistry);
                base.RegisterDefaults(containerRegistry);

                containerRegistry.RegisterSingleton<IBaseWindow, MainWindow>();

                // ...
            }
        }
        ```  
        You also should register the window that would be the main window of the app and the window has to implement ```IBaseWindow``` interface.  

</TabItem>  

<TabItem value="avalonia" label="Avalonia">  

        The first step is to add *PackageReference* to your project:
        ```xml
        <ItemGroup>
            ...
            <PackageReference Include="Hypocrite.Services.Avalonia" Version="SpecifyVersionHere" />
            ...
        </ItemGroup>
        ```

        Change your **App.xaml.cs** file so the ```App``` class whould be inherited from ```ApplicationBase```. The assembly of your project should be registered in ```IViewModelResolverService``` so you should override ```CreateShell``` method. 
        ```csharp
        public partial class App : ApplicationBase
        {
            protected override AvaloniaObject CreateShell()
            {
                var viewModelService = Container.Resolve<IViewModelResolverService>();
                viewModelService.RegisterViewModelAssembly(Assembly.GetExecutingAssembly());
                // ...

                return base.CreateShell();
            }

            protected override void RegisterTypes(IContainerRegistry containerRegistry)
            {
                base.RegisterTypes(containerRegistry);
                base.RegisterDefaults(containerRegistry);

                containerRegistry.RegisterSingleton<IBaseWindow, MainWindow>();

                // ...
            }
        }
        ```  
        You also should register the window that would be the main window of the app and the window has to implement ```IBaseWindow``` interface.  

</TabItem>  

</Tabs>