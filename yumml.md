--- 
layout: page
title: YumML - Yet Another Recipe Metadata Language
description: Yet Another Recipe Metadata Language - you know, for food.
date: 2011-09-21
tags: "food YumML"
---

##Version##
Version: 0.1  
Date: 2011-09-21  
Author: Paul Jenkins  

**Contents**  

- [Version History](#versionhistory)
- [Introduction](#intro)
- [Basic Example](#basicexample)  
- [Extended Example](#extendedexample)  
- [Specification](#specification)  
	- [ID's](#ids)
	- [Header](#recipeheader)
	- [Ingredients](#ingredients)
	- [Method](#method)
- [Packaging](#packaging)  

<h2 id="versionhistory">Version History</h2>
0.1 - initial draft

<h2 id="intro">Introduction</h2>

From [YAML.org](http://www.yaml.org/)
> What It Is: YAML is a human friendly data serialization standard for all programming languages.

YumML (the sound you make when you eat a recipe made from *Yet Another Recipe Metadata Language*), is based on YAML instead of XML to create a human *and* system readable format.  

YumML has three level of detail - **Basic/Required**, **Optional** and **Extended**. The **Basic/Required** is the minimum that YumML recipes and parsers/programs *must* support.  

**Optional** values are as the name implies, optional for recipes and parsers, but recommended for both. Optional values include:  
- duration of each step,  
- an image of the overall product or of each step,  
- what tools are required for the recipe  

While they aren't required, they can make recipes much easier to follow.   
  
**Extended** detail is *metadata* about the recipe, helping link steps to ingredients and which steps can be done in a parallel fashion. While this metadata generally *should not* be exposed to the end user in a YumML application, it is useful for generating graphs in the *Tabular Recipe Notation* style.

<h2 id="basicexample">Basic Example</h2>
This is an abridged recipe, cutting out steps and ingredients. This is by design to show off the *basic* and *required* form of a YumML recipe.  [You can find the full basic example recipe in it's own YumML file][1]

	name: Mrs Fields Choc-Chip Cookies
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

<h2 id="extendedexample">Extended Example</h2>
Like the above basic example, this is an abridged *extended* example. [You can find the full extended example recipe in it's own YumML file][2]

	name: Mrs Fields Choc-Chip Cookies
	date: 2011-09-21
	time: 25 minutes
	ingredient:
	    - qty : 2.5
	      unit: cups
	      item: plain flour
	      id  : 1

	    - qty : .5 
	      unit: tsp 
	      item: bi-carb of soda
	      id  : 2

	    - qty : .25  
	      unit: tsp 
	      item: salt
	      id  : 3

	method:
	    - step: Mix flour, bi-carb soda, and salt in a large bowl
	      uses: 1,2,3 
	      id  : 11

	    - step: Blend sugars with electric mixer, add margarine to form a grainy paste
	      uses: 4,5,6
	      id  : 12


<h2 id="specification">Specification</h2>
The two examples above should demonstrate that there are *three* main sections to a YumML recipe - the header, the ingredients list, and the method (or instruction/steps) listing. 

<h3 id="ids">IDs</h3>
*Extended Only*  
Adding identifiers to the steps and ingredients sections makes each recipe more complex, but creates huge potential for the way to present the data by linking ingredients to steps, and ordering/arranging steps together. In particular, Michael Chu (of Cooking For Engineers) has the fantastic *Tabular Recipe Notation* and creating a data format that can output that table format would be difficult without being able to identify where each ingredient or step comes into the process.  

Here's an example of the *Tabular Recipe Notation* for the above recipe

<table style="border:1px solid #cacaca;">
	<tr>
		<td>flour</td>
		<td rowspan="3">mix</td>
		<td class="empty"></td>
		<td class="empty"></td>
		<td class="empty"></td>
		<td rowspan="10">Add & Stir</td>
		<td rowspan="10">Blend</td>
		<td rowspan="10">Bake @ 160c<br /> for 18-20mins</td>
		<td rowspan="10">Transfer to cooling rack</td>
	</tr>
	<tr>
		<td>bicarb</td>
	</tr>
	<tr>
		<td>salt</td>
	</tr>
	<tr>
		<td>brown sugar</td>
		<td class="empty"></td>
		<td rowspan="3">blend</td>
		<td class="empty"></td>
		<td rowspan="6">Add & Stir</td>
	</tr>
		<tr>
		<td>white sugar</td>

	</tr>
	<tr>
		<td>margarine</td>
	</tr>

	<tr>
		<td>eggs</td>
		<td class="empty"></td>
		<td class="empty"></td>
		<td>beat</td>
	</tr>
	<tr>
		<td>vanilla</td>
	</tr>
	<tr>
		<td>golden syrup</td>
	</tr>
	<tr>
		<td>choc chips</td>
	</tr>

</table>

This is a (relatively) complex example showing steps involving one or more ingredients and some steps having the output of a previous step, but it doesn't show steps being executed simultaneously.

There is no need to restrict identifiers to a particular format - they can be alphanumeric (a->z, 0->9), sequential (1,2,3,4...), random or GUIDs - just so long as they're unique.


<h3 id="recipeheader">Header</h3>

	name: Mrs Fields Choc-Chip Cookies
	date: 2011-09-21
	time: 25 minutes
	serves: 1
	makes: 25
	tags:
 		- cookies
 		- chocolate

- `name`, name of the recipe
- Optional: `date`
- Optional: `time`, how long the recipe should take
- Optional: `serves`, how many people the recipe should serve - best suited for 'meal' recipes  
	OR  
- Optional: `makes`, how many items it makes - best suited for baking (ie, 25 cookies could serve 25 people 1 cookie each, or 1 person with 25 cookies over several days)

<h3 id="ingredients">Ingredients</h3>
	- qty : 2.5
	  unit: cups
	  item: plain flour
	  id: 1

- `qty`, quantity should be listed in decimal format (not fractional) for ease of conversion
- `unit`, quantity without a qualifier (cups, tsp, tbsp, lbs, g, kg, mL, L) means nothing.
- `item`, the actual ingredient name
- Extended: `id`. See IDs

<h3 id="method">Method</h3>
Each step (or instruction) in any recipe has a basic "do foo with bar", but there is further metadata you could attach with each step.

	- step : Mix flour, bi-carb soda, and salt in a large bowl
	  tools: large bowl, spoon
	  image: mixing.jpg
	  brief: mix
	  id   : 4
	  uses : 1,2,3
	  duration : 1
	  timer : 120

- `step` is the full description/instruction for that particular step
- Optional: `tools` are what utensils/tools are being used in that step. This could potentially be used to create a filter of recipes based on what utensils you do or don't have.
- Optional: `image` would represent a relative or absolute path to a photo associated with that step. Depending on the complexity or ambiguity of a recipe, visual aids often help.
- Extended: `id`. See IDs
- Optional: `uses` a list of comma separated *ingredient* ID's. That is for this step, it'd use the ingredients labeled 1, 2 and 3.
- Optional: `duration` should be listed in minutes

<h2 id="packaging">Packaging</h2>
Optionally, YumML recipes combined with photos/images associated with the recipe, could be packaged into a single file which would be just a renamed zip file (.YumZ). Any images referenced in the recipe should be relative paths rather than absolute.   

Is there a need for a packaging standard/format for it? It'd be an easy way to cache it on portable devices if you just had to download a single file then expand, rather than downloading each. It makes the format more portable, but probably isn't a pressing need.


  [1]: /yumml_example_recipe_basic.txt
  [2]: /yumml_example_recipe_extended.txt