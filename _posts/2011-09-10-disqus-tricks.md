--- 
layout: page
title: "Disqus comment counters on static sites"
date: 2011-09-10
---

The major upside of Jekyll is that it is static, but it's also one of the downsides. Any dynamic stuff you need to do needs to be worked into the 'build process' for the site, or alternative using Javascript and be performed on the client end.

Comments are one of those things that must be offloaded to the client end, and Disqus makes this fairly easy to do however, the traditional '*X* comments' on the index page of a blog isn't blindingly obvious.

Thankfully, again, Disqus makes this fairly easy, although I didn't find any particularly good documentation on it, so there was a bit of trial and error.

First, the javascript. Whack this chunk at the bottom of your "recent posts" page.

	<script type="text/javascript"> 
	//<![CDATA[
	(function() {
	    var links = document.getElementsByTagName('a');
	    var query = '?';
	    for(var i = 0; i < links.length; i++) {
	    if(links[i].href.indexOf('#disqus_thread') >= 0) {
	        query += 'url' + i + '=' + encodeURIComponent(links[i].href) + '&';
	    }
	    }
	    document.write('<script charset="utf-8" type="text/javascript" src="http://disqus.com/forums/<YOURSITESHORTNAME>/get_num_replies.js' + query + '"></' + 'script>');
	})();
	//]]>
	</script> 
 
 Next, you need to make a few adjustments to the HTML you're outputting to link to the comments.
 Before where you might have had   
 `<a href="/mypost#comments">Comments</a>`
 
 you need to replace that with 

 `<a href="/mypost#disqus_thread">Comments</a>`

 The `#disqus_thread` portion is important - anything links with that will get picked up by the Javascript, which then sends those to Disqus and gets a list of comments and reactions back.  **There is one important caveat**, there is a limit to how much data can be sent over a GET request to Disqus. What this means is you probably shouldn't send a request for *all* your posts - limited it to 10-20, and you'll be fine.

 To configure the text that Disqus replaces it with, go into the admin panel of your Disqus account.
 
  [1]: http://www.theleagueofpaul.com/jekyll-windows#jekyllcomments