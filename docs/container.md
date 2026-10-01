---
sidebar_position: 5
id: container
title: LightContainer
---

# LightContainer   

### Why should I use LightContainer?  

*Hypocrite.Services* provides You light and fast DI container called *LightContainer*. Here are some features of it:

#### Attribute injections:  

All the registered shite could be resolved via *Injection* attribute (use the attribute only for properties and fields) like this:
```csharp
private class NormalClass
{
    [Injection]
    InjectedClass TestClass { get; set; }
    [Injection]
    AnotherInjectedClass _anotherTestClass;
}
```

#### Constructor injections:    

Parametrised constructors could also be used (this feature is not fully provided by *UnityContainer*). For example after registering and resolving the class:  
```csharp
private class NormalClass
{
    private InjectedClass _testClass;
    private int _a;
    private string _b;

    public NormalClass(InjectedClass testClass, int a, string b = "awd")
    {
        _testClass = testClass;
        _a = a;
        _b = b;
    }
}
```
the *testClass* parameter would be resolved as usual (if it is not registered in the container then an instance of it would be created); the *a* parameter would have **default type** value (for Int32 is 0); the *b* parameter would have its **default parameter** value (in this case is "awd").  

#### Inheritance injections:   

The classed from which Your class is inherited would also be prepared for injections:  
```csharp
private class InjectedClass
{
    internal int A { get; set; }
}

private class BaseClass
{
    [Injection]
    protected InjectedClass TestClass { get; set; }
}

private class NormalClass : BaseClass
{
}
```
So in this case after *NormalClass* registration and resolve, the *TestClass* property would also be injected.  

#### Recursive injections:  

There could be two classes that require injection of each other (this feature is not provided by *UnityContainer*):
```csharp
private class FirstClass
{
    [Injection]
    SecondClass InjectedClass { get; set; }
}

private class SecondClass
{
    [Injection]
    FirstClass InjectedClass { get; set; }
}
```
And this would work as expected!  

### What about speed?  

After some benchmarks on my shity laptop (idk and idc about its parameters) I got the following results:  

  <div style={{textAlign: 'left'}}>
    <img src="/docshome/img/hypocrite/container/cnt1.png" />
  </div> 

Singleton resolve:  

  <div style={{textAlign: 'left'}}>
    <img src="/docshome/img/hypocrite/container/cnt2.png" />
  </div>  

Type resolve:  

  <div style={{textAlign: 'left'}}>
    <img src="/docshome/img/hypocrite/container/cnt3.png" />
  </div> 

But... there is a nuance with injections because they're quite powerful in *LightContainer*:  

  <div style={{textAlign: 'left'}}>
    <img src="/docshome/img/hypocrite/container/cnt4.png" />
  </div> 

Type resolve with constructor injections:  

  <div style={{textAlign: 'left'}}>
    <img src="/docshome/img/hypocrite/container/cnt5.png" />
  </div> 

Type resolve with fields and properties injections:  

  <div style={{textAlign: 'left'}}>
    <img src="/docshome/img/hypocrite/container/cnt6.png" />
  </div> 
