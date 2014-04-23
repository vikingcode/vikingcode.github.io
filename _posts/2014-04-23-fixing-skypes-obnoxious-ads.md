---
layout: page
title: Fixing Skypes obnoxious ads
comments: true
date: 2014-03-23
---

*EDIT: If I'm running Fiddler2, I can see it hitting and failing static.skypeassets.com, and all ads being blocked. If I stop running Fiddler2, sometimes the ads come back. Investigating...*

One of the worst 'features' of Skype 6.10 onwards is the introduction of ads. Ads make free things possible, sure, but when they're ads for the product you're actually already using, they're pretty grating. However, that would be all forgivable, *if* the ads didn't steal focus. From the IM window. As you're writing. Every single IM window has an ad in it for... usually Skype on your phone.  

The fix is simple. Open up `C:\Windows\System32\drivers\etc\hosts`, and add `127.0.0.1 static.skypeassets.com` to the end.

The ads won't load so they won't steal focus. The 'box' doesn't even load properly so the window doesn't change layout on you.
