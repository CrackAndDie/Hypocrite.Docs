---
sidebar_position: 8
id: fody
title: Fody
---

# Fody   

A package that allows You to use ```[Notify]``` attribute in Your code with **Hypocrite.Services** library.  

You can use ```[Nofity]``` attribute on any property You have in Your class but the class has to be inherited from ```BindableObject```.  
To start using this library You should add a package reference to the library in Your project like:
```xml
<PackageReference Include="Hypocrite.Fody" Version="SpecifyVersionHere">
    <PrivateAssets>all</PrivateAssets>
    <IncludeAssets>runtime; build; compile; native; contentfiles; analyzers; buildtransitive</IncludeAssets>
</PackageReference>
```
And create a *FodyWeavers.xml* file in Your project root with following configuration (if You already have one - ```<Hypocrite />``` could be just added there):
```xml
<Weavers xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="FodyWeavers.xsd">
  <Hypocrite />
</Weavers>
```