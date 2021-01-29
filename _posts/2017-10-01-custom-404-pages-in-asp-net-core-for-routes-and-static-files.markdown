---
layout: post
title:  "Custom 404 pages in ASP.NET Core for routes and static files"
date:   2017-10-01
description: Back in February I wrote a post about how to deliver 404 pages while immediately dealing out the correct status message for ASP.NET applications. I gave a hint about ASP.NET Core and have improved my solution since. So, let’s call this post a follow-up in a trilogy of posts.
tags:
 - ASP.NET Core
 - C#
share: true
---
Back in February I wrote a [post]({% link _posts/2017-02-18-the-absolute-proper-way-to-do-404-pages-in-asp-net-mvc.markdown %}) about how to deliver 404 pages while immediately dealing out the correct status message for ASP.NET applications. I gave a hint about ASP.NET Core and have improved my solution since. So, let’s call this post a follow-up in a trilogy of posts.

In that post I described the use of the ‘StatusCodePages’ middleware with re-executing to a different URL. It worked marvelously, but it caught every error possible. I don’t mind the standard message shown for all errors besides ‘Page not found’, since those aren’t hit that much and don’t always require friendly guidance. After some projects I wished to only capture the 404 error because I didn’t want to have logic for other possible statuses in my controller.

# Catching only 404 status code for custom page

First, you should create the page you want to redirect all 404 errors towards. Just as last time, the magic happens in the Configure section of StartUp.cs where you define a custom piece of middleware just before adding MVC to the pipeline.

{% highlight csharp %}
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseStatusCodePages();
    app.UseStaticFiles();

    //Perform middleware for custom 404 page
    app.Use(async (context, next) => {
        await next();
        if(context.Response.StatusCode == 404)
        {
            context.Request.Path = "/404";
            await next();
        }
    });

    app.UseMvcWithDefaultRoute();
}
{% endhighlight %}

We have added ‘UseStatusCodePages’ first so all other status codes would be taken care off in their default way. Our own piece of middleware lets the rest of the pipeline be executed first before reaching back to it. Once the pipeline reaches back into the middleware, it checks what the status code of the response is. When a 404 was hit, it changes the requested path to our custom 404 and lets the underlying middleware (such as MVC) run again to generate the requested custom 404 webpage before continuing.

![ASP.NET Core Customer 404](/assets/images/asp-net-core-custom-404.png "ASP.NET Core Customer 404")

We get an immediate clean result with no funky redirects in between. The status code remained 404, the URL didn’t change and our custom page is shown. Success.

# Showing default 404 message for missing static files

I started using the above method for my projects. But one of my design-colleagues notified me that he also got the full-blown friendly custom 404 page when there was a reference to a static file that was not found. This was certainly not ideal. The message size for a missing static file would be larger than it should and in worst case an ‘inception’ example of a full-blown website shown in an iframe could occur.

We want to show the default small 404 message for missing static files while keeping my custom page for missing routes. To manage this, we have to add a conditional branch to the configuration of the middleware pipeline.

{% highlight csharp %}
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseStatusCodePages();

    //Perform middleware for static files and end processing
    app.MapWhen(
        context => context.Request.Path.HasValue && context.Request.Path.Value.Contains('.'),
        appBuilder =>
        {
            app.UseStaticFiles();
        }
    );

    //Perform middleware for custom 404 page
    app.Use(async (context, next) => {
        await next();
        if(context.Response.StatusCode == 404)
        {
            context.Request.Path = "/404";
            await next();
        }
    });

    app.UseMvcWithDefaultRoute();
}
{% endhighlight %}

We branched off the pipeline using ‘MapWhen’. Do note that the branch never returns into the root pipeline! The branch only happens when the path of the values contains a ‘.’, which indicates the request is made to an actual file. Since we already declared to use status code pages before the branch we’ll only need to specify to use the static files middleware for this request. This also means requests for files never run into the MVC middleware and might have a very minimal response speed increase.

So, creating and managing 404 messaging in ASP.NET Core is still an easy feat, certainly when we use the power of the middleware pipeline!