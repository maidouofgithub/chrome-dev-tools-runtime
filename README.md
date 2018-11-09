# chrome-dev-tools-runtime
Runtime for the [Chrome Developer Protocol](https://developer.chrome.com/devtools/docs/debugger-protocol). Generated by [BaristaLabs\chrome-dev-tools-generator](https://github.com/BaristaLabs/chrome-dev-tools-generator)

Similar to [Puppeteer](https://github.com/GoogleChrome/puppeteer) but more low-level and for .Net Core.
> Updated for Chrome 70!!


### Instructions

You can obtain the runtime from NuGet at the following location: 

https://www.nuget.org/packages/BaristaLabs.ChromeDevTools.Runtime/

To execute the sample cli:

 - Clone
 - Launch Chrome with debugging enabled at port 9223
 - Run

Windows: 
``` bash
$ git clone https://github.com/BaristaLabs/chrome-dev-tools-runtime
$ cd chrome-dev-tools-runtime\ChromeDevToolsRuntimeCLI
$ "C:\Program Files (x86)\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9223
$ dotnet restore
$ dotnet run
```

macOS:

On macOS the behavior is a little different when launching the chrome process.

First, open a new terminal window and execute
``` bash
$ /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9223 --user-data-dir=remote-profile
```

Then clone and run the sample CLI in another terminal window:
``` bash
$ git clone https://github.com/BaristaLabs/chrome-dev-tools-runtime
$ cd chrome-dev-tools-Runtime/ChromeDevToolsRuntimeCLI/
$ dotnet restore
$ dotnet run
```

### Commands

In general, using an instance of a ChromeSession, submit strongly-typed commands via the adapter methods.

``` CS
using (var session = new ChromeSession("ws://...")
{
    await session.Page.Navigate(new Page.NavigateCommand
    {
        Url = "http://www.winamp.com"
    });
}
```

All commands can specify a timeout and are cancellable:
``` CS
using (var session = new ChromeSession("ws://...")
{
    var shouldCancel = new CancellationTokenSource();
    await session.Page.Navigate(new Page.NavigateCommand
    {
        Url = "http://www.winamp.com"
    }, millisecondsTimeout: 60000, cancellationToken: shouldCancel);
    
    shouldCancel.Cancel();
}
```
### Events

Subscribe to events via the Subscribe method

``` CS
using (var session = new ChromeSession("ws://...")
{
    session.Page.SubscribeToFrameNavigatedEvent((e) =>
    {
        Console.WriteLine($"Navigated to {e.Frame.Url}");
    });
}
```

note that you may need to send the associated enable command for a particular domain.
As events are asynchronous, you may need to ensure you program is still running when the event occurs or you may miss it.

### Sample Projects

 - [Skinny Html2Pdf](https://github.com/baristalabs/skinny-html2pdf) - Convert Pages to PDF within a container.
 - [Skrapr](https://github.com/BaristaLabs/skrapr) - Declarative Web Scraping.


