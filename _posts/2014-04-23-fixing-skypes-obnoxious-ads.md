---
layout: page
title: Fixing Skypes obnoxious ads
comments: true
date: 2014-03-23
---

*EDIT: If I'm running Fiddler2, I can see it hitting and failing static.skypeassets.com, and all ads being blocked. If I stop running Fiddler2, sometimes the ads come back. Investigating...*

**EDIT2: Looks like its "fixed", original article only had one address, now has more**

One of the worst 'features' of Skype 6.10 onwards is the introduction of ads. Ads make free things possible, sure, but when they're ads for the product you're actually already using, they're pretty grating. However, that would be all forgivable, *if* the ads didn't steal focus. From the IM window. As you're writing. Every single IM window has an ad in it for... usually Skype on your phone.  

The fix is simple. Open up `C:\Windows\System32\drivers\etc\hosts`, and add the following to the end:

    127.0.0.1 static.skypeassets.com
    127.0.0.1 ssw.live.com
    127.0.0.1 rad.msn.com
    127.0.0.1 g.live.com
    127.0.0.1 ec.atdmt.com
    127.0.0.1 view.atdmt.com
    127.0.0.1 wltoday.ninemsn.com.au
    127.0.0.1 secure.wlxrs.com
    127.0.0.1 cm.ac3.msn.com
    127.0.0.1 m.adnxs.com
    127.0.0.1 sup.live.com

Why so many? Well, originally I thought it was just the initial (skypeassets) url, but that was only true *while* I was running Fiddler. From [Will's smartscreen investigation](http://willhughes.me/20120426/smartscreen-fights-back/), adding the rest of the addresses fixed it properly.

The ads won't load so they won't steal focus. The 'box' doesn't even load properly so the window doesn't change layout on you.
