--- 
layout: page
title: Url Shortening with Goo.gl in C#
description: Url Shortening with Goo.gl in C#, using Hammock and JSON.NET
funnelweb_id: 65
date: 2011-01-13 13:00:00 +11:00
tags: "c# hammock .net google codesnippet "
comments: true
---
Not so long ago, Google [announced the availability of the API][1] for their url shortener - Goo.gl.

The process is pretty straight forward, [the documentation is acceptable][2] (it's an url shortener, not rocket surgery), but here's a quick sample of how to use it in .NET/C#, by taking advantage of [Hammock][3] and [Json.NET][4]. Neither are required, they just make it so much easier.

As I like to do things in an async manner, this is async using an action. To get it syncronous, just use `Request()` instead of `BeginRequest()`. Error handling is deliberately left out.


    public void Shorten(string Url, Action<string> shortenedUrl)
    {
    	var data = Encoding.UTF8.GetBytes("{\"longUrl\": \"" + Url + "\"}");
    	var c = new RestClient()
    				{
    					Authority = "https://www.googleapis.com/urlshortener/v1/"
    				};
    
    	var req = new RestRequest()
    				  {
    					  Path = "url",
    					  Method = WebMethod.Post,
    				  };
    	req.AddHeader("Content-Type", "application/json");
    	req.AddPostContent(data);
    	c.BeginRequest(req, (ireq, res, obj) =>
    						  {
    								  JObject o = JObject.Parse(res.Content);
    								  var x = (string)o.SelectToken("id");
    
    								  if (!String.IsNullOrEmpty(x))
    								  {
    									  shortenedUrl(x);
    									  return;
    								  }
    						  });
    }

You *can* use authenticated requests, and its recommended you use an API, but neither are **required**


  [1]: http://googlecode.blogspot.com/2011/01/google-url-shortener-gets-api.html
  [2]: http://code.google.com/apis/urlshortener/v1/getting_started.html
  [3]: http://hammock.codeplex.com/125
  [4]: http://json.codeplex.com/
