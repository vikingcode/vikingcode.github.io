---
layout: page
title: WebAuthenticationBroker alternative? No.
date: 2012-07-11 13:40:13 +10:00
---

> **This was set to be a story of triumph over a silly API design, but unfortuantely Microsoft have repeated the mistakes of their past, stopping people from making things more awesome.**

`WebAuthenticationBroker` in WinRT is fantastic, if you don't want to deal with the whole opening and closing of browser windows, managing what "state" in the flow you're up to, etc.  Unfortunately, it provides little in the way of customisation, so if you have a website that doesn't render absolutely perfectly, you're stuck. An example is GitHub (their API uses OAuth2)

![](/images/postimages/webbroker.png)

While it isn't completely unusable, it isn't *pretty* nor the best UX. The "solution" according to Microsoft is to navigate your way to the mobile versions of sites. Unfortunately that just isn't an option.

##Could we have FlexibleWebAuthenticationBroker?
###So what do we want to replace?
####UI
To properly replace `WebAuthenticationBroker`, we need to know what it is first. From the UI side, it looks like

![](/images/postimages/webbroker_breakdown.png)

 It's the `WebView` in that breakdown that causes an issue with its fixed width. To replace the UI (simplistically, there are other things at work), we can do:
 
{% highlight csharp %}
Popup p = new Popup()
{
    HorizontalAlignment = HorizontalAlignment.Stretch,
    VerticalAlignment = VerticalAlignment.Stretch,
    Width = Window.Current.Bounds.Width,
    Height = Window.Current.Bounds.Height
};
var g = new Grid()
{
    HorizontalAlignment = HorizontalAlignment.Stretch,
    VerticalAlignment = VerticalAlignment.Stretch,
    Background = new SolidColorBrush(Colors.Red),
    Width = Window.Current.Bounds.Width,
    Height = Window.Current.Bounds.Height
};
g.Children.Add(new WebView());
p.Child = g;
p.IsOpen = true;
{% endhighlight %}

That'll take up the entire screen, and doesn't need to be "attached" to anything, so it can be called from a view model or a view itself.

####Code
`WebAuthenticationBroker` doesn't really have a particular authentication mechanism baked in, but it works great with OAuth2.

	WebAuthenticationResult WebAuthenticationResult = await WebAuthenticationBroker.AuthenticateAsync(WebAuthenticationOptions.None, StartUri, EndUri);
	
Then that result has information like tokens/etc. Unfortunately, that's about all we have to work on, so from a code point of view we just need to mimic that and figure out how it all slots together.

###It can't be done
First off, using async/await it's remarkably easy to replicate that API. Oh, sure, `WebAuthenticationResult` is sealed so we have to make our own, but designing a class to return the same result at the end of the process is easy.

Secondly, should Chromium or Gecko ever be ported to WinRT, using something like [CefSharp](https://github.com/chillitom/CefSharp/) will make it possible to complete this. However, we're not there at this stage.

**Microsoft seem doomed to repeat past mistakes**. No matter what advances the Internet Explorer team make, when it gets integrated into a framework for developers to use, it ends up... poorly.   

In WPF, and later on in WP7, the `WebBrowser` or `WebView` controls are less than desireable. In WPF (just like WinRT), it suffers from "airspace" issues, where you can't overlay elements. In WinRT thats 'solved' by using `WebViewBrush` to temporarily create a non-interactive element from `WebView`. WPF also has the great issue of [making it difficult to disable the "click" sound](https://connect.microsoft.com/VisualStudio/feedback/details/345528/webbrowser-control-in-wpf-disable-sound) when a user navigates (not so useful in an integrated browser control)

WinRT has its own set of issues regarding rendering the web, though. While it isn't *easy* to do it in WPF, in [WinRT you cannot clear the cookies for a `WebView`](http://social.msdn.microsoft.com/Forums/en-US/winappswithcsharp/thread/2051a5a8-7525-4a1a-81fd-c7dba2bab4e5) - this goes against the behaviour of `WebAuthenticationBroker`.   
 
While that alone is enough to scrap the idea, the final nail in the coffin is the eventing - or lack thereof - around navigation. In WinRT, `WebView` has `LoadCompleted` and.. thats it. WPF has `Navigated` but more importantly `Navigating`. What is the difference? `LoadCompleted` only fires after the entire page - including all images and external resources - has loaded. `Navigated` fires when it has "arrived" at the destination, and `Navigating` fires as the `Uri` changes - and is important if you want to intercept the "EndUri" which may/may not be an actual page (if you're doing 'desktop' authentication). You could argue that the solution is just to use an Uri that exists - easy enough - but what if there is a failure along the way? Well, `WebView` does have `NavigationFailed` which returns `WebViewNavigationFailedEventArgs` which *doesn't* contain the address of the failure, just the address of the site that sent it to the failed Uri.

###Progress that was made
While it was a waste of time, I did manage to get significant portions of code completed for this exercise. This code shouldn't be viewed as... well, it probably shouldn't be viewed, but for prototyping it 'works'. It was meant to be cleaned up, until I hit the killers above.

**FlexibleWebAuthView.xaml**
{% highlight xml %}
	<Page
	    x:Class="FlexibleWebAuth"
	    IsTabStop="false"
	    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
	    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
	    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
	    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
	    mc:Ignorable="d">
	
	    <Grid Background="#CA000000" PointerPressed="Cancelled">
	        <Grid Margin="0,100,0,100" Background="White" PointerPressed="Cancelled">
	            <WebView HorizontalAlignment="Center" Width="500" VerticalAlignment="Stretch" x:Name="wv" />
	        </Grid>
	    </Grid>
	</Page>
{% endhighlight %}
**FlexibleWebAuthView.xaml.cs**

{% highlight csharp %}
using System;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Navigation;

public sealed partial class FlexibleWebAuth
{
	public EventHandler CancelledEvent { get; set; }
	public EventHandler UriChangedEvent { get; set; }

	public FlexibleWebAuth()
	{
		InitializeComponent();
		Loaded += FlexibleWebAuth_Loaded;
		wv.LoadCompleted += wv_LoadCompleted;
	}

	void wv_LoadCompleted(object sender, NavigationEventArgs e)
	{
		//bad code, but 'works' for now
		if (UriChangedEvent != null)
			UriChangedEvent.Invoke(e.Uri, null);
	}

	void FlexibleWebAuth_Loaded(object sender, RoutedEventArgs e)
	{
		wv.Width = Width / 2; //while fixed, would make this work better for 'final'
	}

	public void Navigate(Uri uri)
	{
		wv.Navigate(uri);
	}

	public void Cancelled(object sender, RoutedEventArgs e)
	{
		if (CancelledEvent != null)
		CancelledEvent.Invoke(null, null);
	}
}
{% endhighlight %}

**FlexibleWebAuthenticationBroker.cs**
 {% highlight csharp %}
using System;
using System.Threading.Tasks;
using Windows.Security.Authentication.Web;
using Windows.UI;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Media;

public static class FlexibleWebAuthenticationBroker
{
	public static async Task<FlexibleWebAuthenticationResult> AuthenticateAsync(WebAuthenticationOptions options, Uri startUri, Uri endUri)
	{
		TaskCompletionSource<int> tcs = new TaskCompletionSource<int>();

		Popup p = new Popup
		{
			HorizontalAlignment = HorizontalAlignment.Stretch,
			VerticalAlignment = VerticalAlignment.Stretch,
			Width = Window.Current.Bounds.Width,
			Height = Window.Current.Bounds.Height
		};

		var f = new FlexibleWebAuth
		{
			Width = Window.Current.Bounds.Width,
			Height = Window.Current.Bounds.Height
		};
		f.CancelledEvent += (s, e) =>
		{
			tcs.TrySetResult(1);
			p.IsOpen = false;
		};

		f.UriChangedEvent += (s, e) =>
		{
			if (((Uri)s).AbsoluteUri.Contains(endUri.AbsoluteUri))
			{
				tcs.TrySetResult(1);
				p.IsOpen = false;
			}
		};
		p.Child = f;
		p.IsOpen = true;
		f.Navigate(startUri);
		await tcs.Task;
		return new FlexibleWebAuthenticationResult { ResponseStatus = WebAuthenticationStatus.Success };
	}
}

public class FlexibleWebAuthenticationResult
{
	public string ResponseData { get; set; }
	public WebAuthenticationStatus ResponseStatus { get; set; }
}
{% endhighlight %}

**Usage**

	var result = await FlexibleWebAuthenticationBroker.AuthenticateAsync(WebAuthenticationOptions.None, StartUri, EndUri);

