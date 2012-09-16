---
layout: page
published: true
title: Actually setting RichEditBox RightTapped
date: 2012-09-10
---

Somebody deemed it very clever that RichEditBox and TextBox would have the `RightTapped` event settable in XAML but not have it actually wire up to anything. This causes issues when you want to bring up the appbar on right click - standard Windows 8 behaviour.

The solution is awkward at best, but - as reluctant as I am to admit - it does work.
    
    Editor.AddHandler(RightTappedEvent, (RightTappedEventHandler)RichEditBox_Tapped, true);
    
    private void RichEditBox_Tapped(object sender, RightTappedRoutedEventArgs e)
	{	
    	BottomAppBar.IsOpen = true;
	}