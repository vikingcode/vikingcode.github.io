--- 
layout: page
title: ASP.NET MVC Twitter Signin with Hammock
description: How to do twitter signin for ASP.NET MVC apps using Hammock
funnelweb_id: 63
date: 2011-01-03 13:00:00 +11:00
tags: "aspnetmvc twitter codesnippet "
comments: true
---
I'm currently building a web thingie using ASP.NET MVC3, but I don't really want to have to store users credentials - I don't want to be the "weak" point. I'd much rather use OpenID, Twitter, Facebook or any other service that means I'm not responsible for storing peoples passwords.

So, this is a small class to take care of the ["Sign in with Twitter"][1] process using [Hammock][2]. This doesn't actually do any forms auth or custom membership provider - you'll still need to handle that side of it yourself, but the class is basic enough that it would be easy to wrap use it to do such.

Warnings
-------

One caveat is that it currently only returns the users twitter name (ie, aeoth for me) - it would be worthwhile modifying it to return the users ID as well.

    public class TwitterSignin
    {
        private const string BaseUrl = "https://api.twitter.com/oauth/";
        private const string AuthenticateEndpoint = "authenticate";
        private const string RequestTokenEndpoint = "request_token";
        private const string AccessTokenEndpoint = "access_token";
    
        private readonly string _consumerKey;
        private readonly string _consumerSecret;
    
        private IRestClient Client { get; set; }
        private OAuthCredentials Credentials { get; set; }
    
        public TwitterSignin(string consumerKey, string consumerSecret, string callbackUrl)
        {
            ServicePointManager.Expect100Continue = false;
    
            _consumerKey = consumerKey;
            _consumerSecret = consumerSecret;
            Client = new RestClient()
                            {
                                Authority = BaseUrl
                            };
    
            Credentials = new OAuthCredentials
            {
                ConsumerKey = _consumerKey,
                ConsumerSecret = _consumerSecret,
                CallbackUrl = callbackUrl
            };
        }
    
        public string BeginAuth()
        {
            var response = Request(RequestTokenEndpoint);
    
            var r = new Regex("oauth_token=([^&.]*)&oauth_token_secret=([^&.]*)");
            Match match = r.Match(response.Content);
            Credentials.Token = match.Groups[1].Value;
            Credentials.TokenSecret = match.Groups[2].Value;
    
            //This is the url to redirect the user to
            return string.Format("{0}{1}?{2}", BaseUrl, AuthenticateEndpoint, response.Content);
        }
    
        public bool EndAuth(Uri verifierUri, out string screenname)
        {
            var r = new Regex("oauth_token=([^&.]*)&oauth_verifier=([^&.]*)");
            var match = r.Match(verifierUri.AbsoluteUri);
            var verifier = match.Groups[2].Value;

            if (String.IsNullOrEmpty(verifier))
            {
               screenname = String.Empty;
               return false;
            }    

            //Setup the credentials with the results passed in
            Credentials.Type = OAuthType.AccessToken;
            Credentials.Verifier = verifier.Trim();
            Credentials.Token = match.Groups[1].Value;
    
            //Verify that the user is who they say they are
            var response = Request(AccessTokenEndpoint);
    
            r = new Regex("oauth_token=([^&.]*)&oauth_token_secret=([^&.]*)&user_id=([^&.]*)&screen_name=([^&.]*)");
            match = r.Match(response.Content);
    
            if (match.Groups.Count == 5 && !String.IsNullOrEmpty(match.Groups[4].Value))
            {
                screenname = match.Groups[4].Value;
                return true;
            }
    
            screenname = "";
            return false;
        }
    
        private RestResponse Request(string path, Dictionary<string, string> parameters = null, WebMethod method = WebMethod.Post)
        {
            var request = new RestRequest
            {
                Path = path,
                Method = method,
                Credentials = Credentials
            };
    
            if (parameters != null)
                foreach (var p in parameters)
                {
                    request.AddParameter(p.Key, p.Value);
                }
    
            return Client.Request(request);
        }
    }

Usage
-----


    public class AccountController : Controller
    {
        TwitterSignin twitterAuth = new TwitterSignin("<appKey>", "<appSecret>", "<UrlToYourTwitterAuthoriseAction>");
        public ActionResult TwitterLogin()
        {
            var url = twitterAuth.BeginAuth();
            return Redirect(url);
        }
    
        public ActionResult TwitterAuthorise()
        {
            string username;
            if (twitterAuth.EndAuth(Request.Url, out username))
            {
                ViewBag.User = username;
                return View();
            }
            return RedirectToAction("LogOn");
        }
    }

  [1]: http://dev.twitter.com/pages/sign_in_with_twitter
  [2]: http://hammock.codeplex.com/125125
