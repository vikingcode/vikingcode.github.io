---
layout: page
title: Getting a list of all fonts on the system in WinRT
date: 2012-09-07 09:35:05 +10:00
published: true
---

In WPF-land, getting access to the list of system installed fonts is trivial - `System.Windows.Media.Fonts.SystemFontFamilies` - so trivial you could bind directly to that in XAML. Things are a little different in WinRT/Windows 8 - `System.Windows.Media` doesn't exist, and there isn't a `Windows.` namespace to deal with fonts. Instead, you need to use DirectWrite. While that may sound scary at first, SharpDx makes it trivial.

##List of fonts

{% highlight csharp %}
var x = new List<YarrFontFamily>();
var factory = new Factory();
var fontCollection = factory.GetSystemFontCollection(false);
var familyCount = fontCollection.FontFamilyCount;
for (int i = 0; i < familyCount; i++)
{
    var fontFamily = fontCollection.GetFontFamily(i);
    var familyNames = fontFamily.FamilyNames;
    int index;
    if (!familyNames.FindLocaleName(CultureInfo.CurrentCulture.Name, out index))
        familyNames.FindLocaleName("en-us", out index);

    string name = familyNames.GetString(index);
    var internalFont = new YarrFontFamily { 
                        Name = name, 
                    Characters = new List<string>(), 
                    FontVariants = new List<YarrFont>(), };

    for (int y = 0; y < fontFamily.FontCount; y++)
    {
        internalFont.FontVariants.Add(new YarrFont(fontFamily.GetFont(y)));
    }
    x.Add(internalFont);
}

{% endhighlight %}

`YarrFontFamily` and `YarrFont` aren't overly interesting - they're just containers returning properties so XAML can bind properly. If you're super interested, they're up on [GitHub](https://github.com/vikingcode/yarrmaprt).

That will collect all of the fonts and their *variations*. The variations are important - that gives you information about what style and weights are available to a font - whether its light/normal/semibold/bold (weight), or italic/normal/oblique (style).

Once you have the string information, you can just use that inside regular databinding in WinRT

{% highlight xml %}
<TextBlock FontFamily="{Binding ElementName=fonts, Path=SelectedItem.Name}" 
        Text="{Binding ElementName=example, Path=Text}" 
        FontWeight="{Binding Weight}" 
        FontStyle="{Binding Style}"
        FontStretch="{Binding Stretch}"  />
{% endhighlight %}

##List of characters
.NET has `CharacterToGlyphMap` on the `GlyphTypeFace` object, which gives you all the unicode values a particular type has. Unfortunately DirectWrite doesn't have that capability, and the recommended way to do it is to loop over `HasCharacter`

{% highlight csharp %}
var characters = new List<string>();
var count = 65000; //what should this value be?
for (var i = 0; i < count; i++)
{
    if (font.HasCharacter(i))
    {
        characters.Add(char.ConvertFromUtf32(i));
    }
}
{% endhighlight %}

I've chosen an arbitrary number, 65000, as I'm not sure what a good 'maximum' number is. Technically Unicode goes up to 1114111, although I'm not aware of any fonts/characters that are that high.