--- 
layout: page
title: YumML, use YumML, not XML
description: Yet Another Recipe Metadata Language - you know, for food.
date: 2011-09-21
tags: "food yumml"
---

I won't beat around the bush - existing metadata formats for food recipes suck. Alone that would be my typical bold/arrogant claim, so I present my findings on five different formats (RecipeML, Recipe Exchange Markup Language, CookML and RecipeBook XML) to backup that statement, and then my proposed format. There was a sixth format I was looking into - MealMaster - but the recipe structure doesn't seem to be defined anywhere and as far as I know the only program to use it is MealMaster itself.

###RecipeML###
[RecipeML][1] is an XML based format, and this sets a common theme for most of the other recipe metadata formats. XML (and to a lesser degree JSON) are great at machine or system level communication, but they're unwieldy for a human level communication. Build an app on top of the format to read it, and frankly it doesn't matter *that* much at the end of the day, but it still isn't a perfect system. The problem is creating the recipes in the first place - you either have to generate XML by hand, or use a form to do it which is painful (just for the ingredients list, it would have to be a series of dropdowns for units, lots of add buttons, etc).

RecipeML has been around since 2000, and that age gives it a huge amount of recipes available in that format, which is certainly it's strong point. The format itself isn't badly designed for system communication - it's just when you involve squishy humans that it fails. In all fairness, it hasn't been [updated since 2002][3].

From the [examples][2] page

	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE recipeml PUBLIC "-//FormatData//DTD RecipeML 0.5//EN" "http://www.formatdata.com/recipeml/recipeml.dtd">
	<?xml-stylesheet href="dessert1.css" type="text/css"?>
	<recipeml version="0.5">
	  <recipe>
	    <head>
	      <title>The Needless-Markman Hoax Chocolate-Chip Cookie</title>
	    </head>
    	<ingredients>
	      	<ing>
	        	<amt><qty>2</qty><unit>cups</unit></amt>
	        	<item>butter</item>
	      	</ing>
		  	<!-- snip -->
	    </ingredients>
	    <directions>
	      	<step>Measure oatmeal and blend in a blender to a fine powder</step>
			<!-- snip -->
	    </directions>
	  </recipe>
	</recipeml>

It is a verbose format, but there are things to like about it - separating out quantity and unit for each ingredient makes it easy to translate between imperial and metric.
	
###The Recipe Exchange Markup Language###
From the REML project page
> I might have developed against an existing markup languages, but RecipeML is mired in licensing problems, and CookML is written in German. 

My reasons for not using CookML are below, but the point about RecipeML is... interesting. I don't know if it does actually have licensing issues - I don't care, but if it is true, it's a good reason not to use it. 

What of REML itself? From the sample only found inside a zip file on the project page, listed the ingredients like so:

	<ingredient name="Corn flakes" quantity-integer="1">
		<IUnit>
			<IUnitArbitrary>box</IUnitArbitrary>
		</IUnit>
	</ingredient>

In addition, I couldn't find anything other than the one sample recipe, so it could very well be a dead format.

###CookML###
I couldn't find an actual example recipe of [CookML][4], but I don't think I need to.

- It's XML based
- if you thought RecipeML was verbose, the documentation for CookML is at least twice the size
- Unlike RecipeML it doesn't seem to have a huge wealth of recipes available

###RecipeBook XML###
Already the title gives away the main issue - it's XML - but [RecipeBook XML][5] seems very similar to RecipeML by only adding the equipment used, yet dropping the step definition and lumping all instructions in one block.
	
##YumML (or Yet Another Recipe Metadata Language)##
**Design Considerations**  
- I wanted a format that didn't need a long reference guide (which certainly cut out CookML!)  
- I wanted something that could be easily read by non-technical people in the 'raw' format, or via an app  
- I wanted something that could translatable between imperial and metric (in Australia, we use cups for measuring although everything is still metric, yet other places *just* use metric weights or volumes)
- Ideally I wanted the *markdown* of recipes, while maintaining some form of required structure for easy parsing for apps

From [YAML.org](http://www.yaml.org/)
> What It Is: YAML is a human friendly data serialization standard for all programming languages.

YumML is based on YAML instead of XML to create a human *and* system readable format.  

**Basic Example**  
This only includes two ingredients, and two steps. This is deliberate to try and keep this post from being massive - see further down for the full recipe.  

	name: Mrs Fields Choc-Chip Cookies
	date: 2011-09-21
	time: 25 minutes
	ingredient:
	    - qty : 2.5
	      unit: cups
	      item: plain flour

	    - qty : .5 
	      unit: tsp 
	      item: bi-carb of soda

	method:
	    - step: Mix flour, bi-carb soda, and salt in a large bowl
	    - step: Blend sugars with electric mixer, add margarine to form a grainy paste

If you compare this against any XML based format, it's cleaner and easier to read and write, while still maintaining the ability to convert each unit into another form. There is also provision for more detailed

**Full Specification**  
[I've split the YumML specs off into a separate page to avoid the ranty bit's on the top of this blog post][6].

**Recipes**  
There isn't a central repository of YumML recipe's just yet - if there is any interest, I'll build one but first I'm looking at developing various tools for YumML, such as YumML2Html to generate the Tabular Recipe Notation table from [CookingForEngineers][7] and display clients for WP7 & Windows with voice controls.

[Mrs Fields Choc Chip Cookies (Basic YumML)][8]  
[Mrs Fields Choc Chip Cookies (Extended YumML)][9]  


  [1]: http://www.formatdata.com/recipeml/
  [2]: http://www.formatdata.com/recipeml/examples.html
  [3]: http://www.formatdata.com/recipeml/spec/recipeml-spec.html
  [4]: http://www.kalorio.de/index.php?Mod=Ac&Cap=CE&SCa=../cml/CookML_EN
  [5]: http://www.happy-monkey.net/recipebook/
  [6]: /yumml
  [7]: http://www.cookingforengineers.com/
  [8]: /yumml_example_recipe_basic.txt
  [9]: /yumml_example_recipe_extended.txt