---
layout: post
title:  "Perform Javascript from codebehind with RegisterStartupScript"
date:   2014-05-17
description: A piece of javascript can be formed within codebehind and registered to run after the page has loaded.
tags:
 - ASP.NET
 - VB
 - C#
 - Javascript
share: true
---
In a website, you might want to perform javascript after the postback to enhance the user experience of the visitor. Of course you’re able to perform the javascript within the document ready state of the page. But you might want to differ the function called, based on what the visitor filled into the form that has been posted.

Then you can still register a piece of javascript in the code-behind with the help of [ClientScriptManager.RegisterStartupScript](http://msdn.microsoft.com/en-us/library/z9h4dk8y(v=vs.110).aspx). This piece of javascript will then be executed after the page has loaded. I use the snippet below to navigate to an anchor after the postback of a form.

>VB
{:.filename}
{% highlight vb %}
Page.ClientScript.RegisterStartupScript(Me.[GetType](),
           "anchor", "location.hash = '#contact';", True)
{% endhighlight %}

>C#
{:.filename}
{% highlight csharp %}
Page.ClientScript.RegisterStartupScript(this.GetType(),
           "anchor","location.hash = '#contact';",true);
{% endhighlight %}