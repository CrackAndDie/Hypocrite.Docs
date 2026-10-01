---
sidebar_position: 2
id: localization
title: Localization
---

# Localization   

**Hypocrite.Services** also provides you a great realtime localization solution. To use it you should create a folder ```Localization``` in your project and make some changes in your *App.xaml.cs* file:  

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>  

<TabItem value="wpf" label="WPF" default>    

```csharp
public partial class App : ApplicationBase
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // initialization of LocalizationManager static service
        LocalizationManager.InitializeExternal(Assembly.GetExecutingAssembly(), new ObservableCollection<Language>()
        {
            new Language() { Name = "EN" },
            new Language() { Name = "RU" },
        });  
        // ...
        base.OnStartup(e);
    }
}
```  

</TabItem>  

<TabItem value="avalonia" label="Avalonia">  

```csharp
public partial class App : ApplicationBase
{
    public override void Initialize()
    {
        // initialization of LocalizationManager static service
        LocalizationManager.InitializeExternal(Assembly.GetExecutingAssembly(), new ObservableCollection<Language>()
        {
            new Language() { Name = "EN" },
            new Language() { Name = "RU" },
        });  
        // ...
        base.OnStartup(e);
    }
}
```  

</TabItem>  

</Tabs>   

The ```LocalizationManager.InitializeExternal``` method gets an assembly where to search for *.resx* files as the first parameter. 
And all available languages for you project as the second parameter.  

:::note
Languages using ```LocalizationManager.InitializeExternal``` method could only be initialized once.
:::

### Creating *.resx* tables

In the ```Localization``` folder you should create Resource files with translations and call it as follows - "FileName"."Language".resx (*Gui.resx* or *Gui.ru.resx*). Default resource file doesn't need to have the "Language" part.  
  
### Usage in *xaml*

Now you can use it like this:  

```xml
<TextBlock Text="{LocalizedResource MainPage.TestText}"
           Foreground="{DynamicResource TextForegroundBrush}" />
```  

Or via *Bindings* to a ViewModel:  

```xml
<TextBlock Text="{LocalizedResource {Binding TestText}}"
           Foreground="{DynamicResource TextForegroundBrush}" />
```  

### Changing current language

To change current localization use ```LocalizationManager.CurrentLanguage``` property like this:
```csharp
LocalizationManager.CurrentLanguage = CultureInfo.GetCultureInfo(lang.Name.ToLower());
```
In this example *lang* is an instance of ```Language``` class.  

:::warning
Chaning ```LocalizationManager.CurrentLanguage``` also changes ```Thread.CurrentThread.CurrentUICulture```
:::

### Working with localization from code

To get translation from code you can use ```GetValue``` method:
```csharp
string translated = LocalizationManager.GetValue("TestKey");
```
By default ```GetValue``` method searches for the translations in *Gui._.resx* files.   
You can specify the table (*.resx* file) to search for the translation:  
```csharp
string translated = LocalizationManager.GetValue("SimulatorTranslations", "TestKey");
```  

### Custom localization providers  

If you want to add custom localization provider (not only using *.resx* files) you can implement interface ```ILocalizationProvider```. 
Here is an example of how to do it:  
```csharp
public class AdditionalLocalizationProvider : ILocalizationProvider
{
    public AdditionalLocalizationProvider(IList<AdditionalTranslation> additionalTranslations) 
    {
        Dictionary<string, object> enDic = new Dictionary<string, object>();
        Dictionary<string, object> ruDic = new Dictionary<string, object>();
        Dictionary<string, object> zhDic = new Dictionary<string, object>();
        Dictionary<string, object> jaDic = new Dictionary<string, object>();
        foreach (var translation in additionalTranslations)
        {
            enDic.Add(translation.Tag, translation.En);
            ruDic.Add(translation.Tag, translation.Ru);
            zhDic.Add(translation.Tag, translation.Zh);
            jaDic.Add(translation.Tag, translation.Ja);
        }
        allResources.Add("en", enDic);
        allResources.Add("ru", ruDic);
        allResources.Add("zh", zhDic);
        allResources.Add("ja", jaDic);
    }

    public object GetValue(string key, CultureInfo culture = null)
    {
        if (key is null)
        {
            return null;
        }

        var dictionary = GetValues(culture);
        if (dictionary.TryGetValue(key, out var value))
        {
            return value;
        }

        return null;
    }

    public Dictionary<string, object> GetValues(CultureInfo culture = null)
    {
        if (culture == null)
        {
            culture = NeutralCulture;
        }

        if (allResources.TryGetValue(culture.IetfLanguageTag, out var dictionary))
        {
            return dictionary;
        }

        return new Dictionary<string, object>();
    }

    public static CultureInfo NeutralCulture { get; } = CultureInfo.GetCultureInfoByIetfLanguageTag("en");

    private readonly Dictionary<string, Dictionary<string, object>> allResources = new Dictionary<string, Dictionary<string, object>>();
}
```  

And register it like that:
```csharp
LocalizationManager.AddScopedProvider("Gui", new AdditionalLocalizationProvider(translations));
```  

:::note
You can register multiple ```ILocalizationProvider``` implementations under the same name
:::
