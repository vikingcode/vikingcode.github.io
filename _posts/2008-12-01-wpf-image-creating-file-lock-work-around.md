--- 
layout: page
title: WPF Image creating file lock work around
description: WPF Image creating file lock work around
funnelweb_id: 28
date: 2008-12-01 13:00:00 +11:00
tags: ".net programming wpf "
comments: true
---
While working on a new version of TVScout, I've been working on adding a management tool which will allow you to choose and fetch from the variety of posters on thetvdb or themoviedb, not just the first poster found. The problem is, I'm displaying the current poster in an (System.Windows.Controls.) Image first. This creates a situation where my program wants to overwrite a resource that's already in use.The simple fix would be set the Image.Source to null first? Partially right, but the problem is with how you set the file in the first place as there there is still a file lock.

The solution I found was a weird one to say the least. You have to make sure caching is enabled so that WPF will load the image into memory, and release the file lock and then tell it to always ignore the cache. Yes, you read that right.

*You have to use the cache and then ignore the cache.*

If you don't ignore the cache, it won't refresh the image to the new image until the next launch of the application.

    private void SetImage(String filename)
    {
    	BitmapImage bi = new BitmapImage();
    	bi.BeginInit();
    	bi.CacheOption = BitmapCacheOption.OnLoad;
    	bi.CreateOptions = BitmapCreateOptions.IgnoreImageCache;
    	bi.UriSource = new Uri(filename);
    	bi.EndInit();
    	image.Source = bi;
    }

So you'll end up with something like this in your code

    SetImage("YourImage.jpg");
    GetNewImage();Image.Source = null;
    SaveNewImage("YourImage.jpg");
    SetImage("YourImage.jpg");


