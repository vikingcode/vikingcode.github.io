---
layout: page
title: YarrMap for WinRT
permalink: slace-is-a-jerk.html
date: 2012-10-17
---

While we all know that [Aaron is a jerk](https://twitter.com/slace/status/243564223416918017) what you may not know is that you shouldn't challenge me to more code. [YarrMap](charmap.html) will work great on Windows 8, but not inside *Windows RT* mode/devices.

![](http://vikingco.de/YarrMapRT/img/tablet_3.png)

Well, as of today, [YarrMap is now available for Windows 8/RT on the Windows Store](http://apps.microsoft.com/webpdp/en-us/app/d68952e8-f4bc-4c74-a108-458b4f891f72).

"YarrMap RT" isn't as complete as I'd like it to be, but it it is relatively functional. You know, for a character map. Like the desktop [YarrMap](/charmap.html), it is able to render far more glyphs than charmap.exe. Unlike the desktop version, it may not render *all* glyphs, though, as several APIs are just not there. For example, there isn't a "C#" API for getting a list of system fonts, instead you need to dive into DirectWrite (SharpDX provides a wonderful managed wrapper for it).

###Getting a list of fonts on the system
{%highlight csharp%}
var x = new List<string>();
var factory = new Factory();
FontCollection fontCollection = factory.GetSystemFontCollection(false);
int familyCount = fontCollection.FontFamilyCount;
for (int i = 0; i < familyCount; i++)
{
    FontFamily fontFamily = fontCollection.GetFontFamily(i);
    LocalizedStrings familyNames = fontFamily.FamilyNames;
    int index;
    if (!familyNames.FindLocaleName(CultureInfo.CurrentCulture.Name, out index))
        familyNames.FindLocaleName("en-us", out index);

    string name = familyNames.GetString(index);
    x.Add(name);
}
Fonts = new ObservableCollection<string>(x.OrderBy(y => y));
{%endhighlight %}

###Getting a list of glyphs for a font
In .NET, you can get the list of actual glyphs for a font. Even with DirectWrite, you can't do that, you *only* have access to `font..HasCharacter(i)`, where i is an int, and there isn't an easy-to-determine upper limit for a particular font. Just because it doesn't have anything in the 1000-2000 range, doesn't mean that the 3000-4000 range is 'empty'. I've made a compromise and only check for the first 65,000.

Overall YarrMap RT was...interesting to write. Far more challenging than I originally thought, which lead me to figure out the best way to get [1000s of items displayed in a grid from my previous post](/gridviewsucks.html)