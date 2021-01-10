---
layout: post
title:  "Execute code for a page after Master Page is loaded"
date:   2013-06-05
description: Sometimes you’d like to edit a public property or call a function of a Master Page from the currently requested page.
---
The ASP.Net Master Page is very useful for many websites. Here you can define components of your website that will be shown on all pages using that particular Master Page. Examples of these kind of components are ‘Footers’, ‘Headers’, ‘Navigation’, …

But sometimes you’d like to edit a public property or call a function of that Master Page from the currently requested page. This could be, for example, to change the title or social meta-tags on the page to something item-specific.

The requested page’s ‘Page_Load’ function is always called first. After that, the ‘Page_Load’ functions of the controls that are used by the page are called. On a page, the Master Page is also considered a control. Which means the ‘Page_Load’ of the Master Page is called later than that of the page.

So how do we interact with the Master Page after it has completed it’s ‘Page_Load’, but when nothing is triggered (like a button)? There are other functions that are performed by the page after the controls have been loaded. The functions to be used best are ‘OnLoadComplete’ and ‘OnPreRender’. I prefer to keep these things in ‘OnLoadComplete’. So you can add your ‘after load’-logic to this function for it to be performed when the Master Page is loaded.

{% highlight csharp %}
protected override void OnLoadComplete(EventArgs e)
{
    (Master)Master.NavigationTitle = "Test";
    base.OnLoadComplete(e);
}

protected override void OnPreRender(EventArgs e)
{
    (Master)Master.SocialTitle = "Test";
    (Master)Master.GenerateSocialMetaTags();
    base.OnPreRender(e);
}
{% endhighlight %}

{% highlight vb %}
Protected Overrides Sub OnLoadComplete(e As EventArgs)
    CType(Page.Master, Master).GenerateSocialMetaTags()
    MyBase.OnLoadComplete(e)
End Sub
{% endhighlight %}