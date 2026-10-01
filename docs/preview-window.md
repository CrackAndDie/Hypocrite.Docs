---
sidebar_position: 7
id: preview-window
title: Preview window
---

# Preview window  

You can create your own preview window using **Hypocrite.Services**. Your preview window has to implement ```IPreviewWindow``` interface. Here is the sample preview window that shows up for 4 seconds:
```csharp
public partial class PreviewWindowView : Window, IPreviewWindow
{
    private DispatcherTimer timer;
    public PreviewWindowView()
    {
        InitializeComponent();

        timer = new DispatcherTimer()
        {
            Interval = TimeSpan.FromSeconds(4),
        };
        timer.Tick += (s, a) => { CallPreviewDoneEvent(); };
        timer.Start();
    }

    public void CallPreviewDoneEvent()
    {
        timer.Stop();
        EventAggregator.GetEvent<PreviewDoneEvent>().Publish();
        this.Close();
    }

    [Injection]
    IEventAggregator EventAggregator { get; set; }
}
```

Registered Window under ```IBaseWindow``` interface will be shown up after ```PreviewDoneEvent``` event call.
