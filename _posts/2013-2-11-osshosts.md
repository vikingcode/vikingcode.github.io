---
title: OSS/DVCS provider critique
layout: page
published: false
---

While I might not be contributing to the OSS world any more, I feel more free to criticise one of the most important factors of OSS - the (D)VCS hosting providers. The main players are SourceForge, CodePlex, Bitbucket and GitHub. Yes, there *are* others, but they simply don't have as large an audience. 

In terms of DVCS, Git is the winner. It doesn't matter if it isn't as user friendly or if you prefer Hg/Bzr/etc, Git has *won*. Personally I have mixed feelings about it, but just like how CVS faded away for SVN, Git has come out on top. All the major players have adopted it - even BitBucket which held onto Hg for as long as possible, hell, even *TFS* now has Git.

###Similarities
It's important to establish a baseline of what is similar between these four.

- Free - they're all free. Yes, Bitbucket/GitHub have the option to give them money for various things, but that is *optional*
- Git - they all offer git. Yes, even SourceForge, known for CVS/SVN offers Git (and Hg and freaking Bazaar!)

###Orientation
GitHub/Bitbucket are user-oriented, CodePlex/SourceForge are project-oriented. Perhaps it is the difference between "Web1" and "Web2.0" mentalities, perhaps it is the focus on social coding over 'just getting it done', I don't know, but the way the provider is oriented makes a huge difference to how they're perceived. 

###Display
Displaying a project is a pretty important thing - 

Here's the odd thing - this is where the project-oriented aren't lagging behind. In some respects they're actually making it easier than the two user-oriented options.

###Discovery
Bizarrely again, SourceForge leads the way (with CodePlex behind it) in discovering new projects. You *could* search on GitHub/Bitbucket to find what you want, but SourceForge actively promotes and recommends projects - the landing page has a "Project of the Month", and browsing categories lets you narrow that down.

CodePlex has a similar 'most downloaded' and 'popular releases' on the landing page, as well as 'related projects' on the individual project landing pages.  

At the *very end* of their self promotion, BitBucket has a few big name projects that they host listed.

GitHub has... well, it has a link to the 'Explore' page which shows you 5 trending repos and 5 featured repos. Being focussed on the user has left GitHub with little 'customisation' for project metadata - a title, a short one line description and that is it. No categories (other than what language it is written in), no icons, no 'we'd really like some contributors'.

Discovery is *always* going to be a big issue in OSS and it plays a major role in why there are so many duplicate projects (yes, politics, style, and 'just because' also play a role).

###Does it matter?
Does it matter if you're technically the best? What if you're all design and little substance? What if you're horribly slow but do everything else right?

At the end of the day, two things matter for OSS - code and people. From *personal* experience with projects hosted on CodePlex, BitBucket and GitHub (sometimes the same one migrated) is that GitHub wins in people - the amount of issues raised, pull requests received, etc far outweighed any other host I've tried. That sees more code moved over, and more people.

So what does GitHub get right? Their interface - while changing every other week - is good enough (unlike CodePlex) and doesn't overwhelm with too much noise (unlike SourceForge). BitBucket comes close to the interface.