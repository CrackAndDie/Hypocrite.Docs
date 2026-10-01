---
sidebar_position: 6
id: observer
title: Observer pattern
---

# Observer pattern   

**Hypocrite.Services** provides You some methods to observe property changes in a bindable class. An example:
```csharp
this.WhenPropertyChanged(x => x.BindableBrush).Subscribe((b) =>
{
    Debug.WriteLine($"Current brush is: {b}");
});
```  

Where ```BindableBrush``` is declared as:
```csharp
[Notify] // would work only with Hypocrite.Fody 
public SolidColorBrush BindableBrush { get; set; }
```  

:::note
You should use [DynamicData](https://github.com/reactivemarbles/DynamicData) or [ReactiveUI](https://github.com/reactiveui/ReactiveUI) that provide more powerful work with *Observer* pattern.
:::
