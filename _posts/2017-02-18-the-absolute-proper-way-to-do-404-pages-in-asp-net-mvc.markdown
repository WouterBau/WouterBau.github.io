---
layout: post
title:  "The absolute proper way to do 404 pages in ASP.Net MVC"
date:   2017-02-18
description: Websites and webservices should return the proper HTTP status codes as response to requests, along with the possible proper body. When you’re a using ASP.Net MVC web application it can be a little tricky because of a configuration quirk I quite dislike and didn’t find much info about. They improved this practice immensely in ASP.Net Core web applications. So, I want to make this post to clear it up and get it all in one nice explanation and solution.
tags:
 - ASP.NET MVC
 - ASP.NET Core
 - C#
share: true
---
Websites and webservices should return the proper HTTP status codes as response to requests, along with the possible proper body. When you’re creating a ASP.Net MVC web application it can be a little tricky because of a configuration quirk I quite dislike and didn’t find much info about. They improved this practice immensely in ASP.Net Core web applications. So, I want to make this post to clear it up and get it all in one nice explanation and solution. This post is an improvement to my previous [post]({% link _posts/2013-11-11-404-pages-in-asp-net-websites.markdown %}) about 404 pages with classic aspx pages.

# ASP.NET Web Applications
Let’s say we have a regular ASP.Net MVC project with a page that lists some items and has a detail page of an item. When users request a detail page of an item, we’ll check the id they provided. We’ll return a ‘404 Not Found’ when no item has the requested id. To make it user friendly, we provide a 404-page fitting in our layout view. We also want to show that 404-page when the visitor attempts to navigate to an URL that is not routed to an action.

This could be the controller containing all actions of the application (Note the change of status code in NotFound):

>C#
{:.filename}
{% highlight csharp %}
public class HomeController : Controller
{
    public ActionResult Index()
    {
        return View();
    }
    
    public ActionResult About(string id = "")
    {
        switch(id){
            case "":
                ViewBag.Message = "Your application description page.";
                break;
            case "1":
                ViewBag.Message = "Your application description page with id 1.";
                break;
            default:
                return HttpNotFound();
        }            
        return View();
    }
    
    [Route("404")]
    public ActionResult NotFound()
    {
        Response.StatusCode = 404;
        return View();
    }

}
{% endhighlight %}

The web.config would contain this snippet to configure the redirection of the result to that of the NotFound action:

>web.config
{:.filename}
{% highlight xml %}
<system.web>
  <customErrors mode="On">
    <error statusCode="404" redirect="/404" />
  </customErrors>
  <compilation debug="true" targetFramework="4.5.2" />
  <httpRuntime targetFramework="4.5.2" />
</system.web>
<system.webServer>
  <httpErrors errorMode="Custom">
    <remove statusCode="404" />
    <error statusCode="404" path="/404" responseMode="ExecuteURL" />
  </httpErrors>
</system.webServer>
{% endhighlight %}

This seems correct, but let’s test this and see what HTTP status codes we receive while we navigate to various nonexistent pages:

![MVC 404 1](/assets/images/MVC_404_1.png "MVC 404 1")

That ‘302 Found’ fits wrong there. Some clients might figure out the next page is a 404, but it isn’t supposed to happen to my feeling. The HttpNotFound() result on a nonexistent id got captured by the webserver who executed the given URL and returned its response. The unknown route however is caught by ASP.Net and it just redirects on a custom error by default, hence the 302. So, we change the custom errors to rewrite instead:

>web.config
{:.filename}
{% highlight xml %}
<customErrors mode="On" redirectMode="ResponseRewrite">
    <error statusCode="404" redirect="/404" />
</customErrors>
{% endhighlight %}

The result is even worse:

![MVC 404 2](/assets/images/MVC_404_2.png "MVC 404 2")

A ‘500 Internal Server Error’ happened! But why? Because customErrors uses [Server.Transfer](https://msdn.microsoft.com/en-us/library/y4k58xk7(v=vs.110).aspx) to pass on the information to the page it rewrites to and Server.Transfer only accepts paths to real existing files. So, it can’t resolve routes and goes into exception. How do we fix this? We give it a path to an actual aspx file:

>web.config
{:.filename}
{% highlight xml %}
<customErrors mode="On" redirectMode="ResponseRewrite">
    <error statusCode="404" redirect="/404.aspx" />
</customErrors>
{% endhighlight %}

In that aspx file, we transfer the request to the correct URL with [Server.TransferRequest](https://msdn.microsoft.com/en-us/library/aa344902(v=vs.110).aspx):

>ASP
{:.filename}
{% highlight csharp %}
<%@ Page Language="C#" AutoEventWireup="true" %>

<% HttpContext.Current.Server.TransferRequest("/404", false); %>
{% endhighlight %}

When we navigate to the non-existing route now, we immediately get the result we hoped for along with a nice HTML response body to give a user-friendly 404-page:

![MVC 404 3](/assets/images/MVC_404_3.png "MVC 404 3")

# ASP.NET Core Web Applications
(I improved my solution in the meantime. Please refer to my next [post]({% link _posts/2017-10-01-custom-404-pages-in-asp-net-core-for-routes-and-static-files.markdown %}).)

Now .Net Core has come along and made things a whole lot simpler in ASP.Net Core Web Applications. We can use almost the same controller for this example:

>C#
{:.filename}
{% highlight csharp %}
public class HomeController : Controller
{
    public IActionResult Index()
    {
        return View();
    }

    public IActionResult About(string id = "")
    {
        switch (id)
        {
            case "":
                ViewBag.Message = "Your application description page.";
                break;
            case "1":
                ViewBag.Message = "Your application description page with id 1.";
                break;
            default:
                return NotFound();
        }
        return View();
    }

    public ActionResult StatusPage(int id = 0)
    {
        ViewBag.StatusMessage = $"status happened: {id}";
        return View();
    }
{% endhighlight %}

To complete things, we just add one piece of middleware to our pipeline in StartUp.cs which will execute the request again to another URL when the response has another status code while maintaining that status code:

>C#
{:.filename}
{% highlight csharp %}
app.UseStatusCodePagesWithReExecute("/home/statuspage/{0}");
{% endhighlight %}

And there we go, perfect status page with perfect response. It couldn’t be simpler.
(I heard about this when a [video](https://channel9.msdn.com/Shows/Web-Hack-Wednesday/Creating-404-middleware-in-ASPNET-core) of Channel 9 was shared by MSDN a few days ago. I figured it would be perfect to include and complete the package. Credit goes to them for this bit.)

These are what I currently experience as the absolute proper ways to do custom status code pages in ASP.Net web applications. I hope this guide has helped you well on track to creating a perfect website responding with all the correct HTTP response codes.