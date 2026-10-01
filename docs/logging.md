---
sidebar_position: 4
id: logging
title: Logging
---

# Logging   

To log your app's work you can resolve ```ILoggingService``` that is just an adapter of ```Log4netLoggingService``` or use ```LoggingService``` property of ```ViewModelBase```.  

You can find the log file in you running assembly directory called *app.log* (or set your own but this requires manual registration of the service).  

You can create your own implementation of ```ILoggingService``` and register it.   

:::note
If you implement your own ```ILoggingService``` you should remove ```base.RegisterDefaults(containerRegistry);``` line in *App._xaml.cs* file and register ```IViewModelResolverService``` and ```IWindowProgressService``` manually like that:
```csharp
containerRegistry.RegisterInstance<ILoggingService>(new YourLoggingService()); // use singleton or instance registration if needed
containerRegistry.RegisterSingleton<IViewModelResolverService, ViewModelResolverService>();
containerRegistry.RegisterSingleton<IWindowProgressService, WindowProgressService>();
```
:::
