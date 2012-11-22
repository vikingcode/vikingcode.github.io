--- 
layout: page
title: Url Shortening with WCF Web APIs HttpClient
description: Url Shortening with WCF Web APIs HttpClient
funnelweb_id: 66
date: 2011-01-14 13:00:00 +11:00
tags: "wcf .net codesnippet "
comments: true
---
Following yesterdays post on [Goo.gl url shortening in C#][1], I've been looking into the [WCF Web API's][2] which saw another release yesterday (as did 90% of the Microsoft web stack!)

So why use WCF Web API's/HttpClient? For a start, [Glenn Block][3] is involved in it. He's one of the wizards involved in MEF (Managed Extensibility Framework) - thats a good enough reason for me, but if that doesn't win you over, eventually this stuff will make it into .NET releases, meaning one less prereq.

What is HttpClient? If you're used to Hammock, its the equivalent of `RestClient`, but if you're not familiar with Hammock, essentially it is a far easier and better way of connecting and consuming to standard REST web services, rather than having to deal with HttpWebRequest or WebClient yourself.

    using System;
    using System.Json;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using Microsoft.Json;
    
    ... 

    public void Shorten(string Url, Action<string> shortenedUrl)
    {
        var c = new HttpClient("https://www.googleapis.com/urlshortener/v1/");
        var req = new StringContent("{\"longUrl\": \"" + Url + "\"}");
        req.Headers.ContentType = new MediaTypeHeaderValue("application/json");
                
        var task = c.PostAsync("url", req);
        task.ContinueWith((t) =>
                            {
                                var shortUrl = ((JsonPrimitive)t.Result.Content.ReadAsJsonValue().GetValue("id")).Value.ToString();
                                shortenedUrl(shortUrl);
                            });
    }

Quick disclaimer - this is the first time I've used HttpClient, so there is a good chance I'm doing something wrong. 

My first thought is *"wow, this is actually very easy to use and pretty 'short' in terms of LoC"*. It doesn't, however, replace Hammock for some of the more advanced web/REST queries - particularly around authentication (OAuth) or streaming. Whether that is on the roadmap or not, I don't know, but it could be awesome.

  [1]: /url-shortening-with-google-in-csharp
  [2]: http://wcf.codeplex.com/
  [3]: http://twitter.com/gblock
