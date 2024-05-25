---
layout: post
title:  "404 pages in ASP.Net websites"
date:   2013-11-11
description: Creating a 404 page for an ASP.Net website isn’t hard. But it depends on when you want it to be showed and how it should be showed. I’ll explain the quick and simple way.
tags:
 - ASP.NET
 - VB
 - C#
 - .NET
share: true
---
You can either create a static ‘404.html’ or a dynamic ‘404.aspx’ page. Afterwards you’ll have to register this page as the error-page for the 404-code in the web.config file of your website. This can be done by adding the <CustomErrors> tag to <system.web>:

{% highlight xml %}
<system.web>

    <customErrors mode="RemoteOnly">
      <error statusCode="404" redirect="/404.aspx" />
    </customErrors>
{% endhighlight %}

‘[CustomErrors](http://msdn.microsoft.com/en-us/library/h0hfz6fc(v=vs.85).aspx)‘ can be used for showing custom error pages for the corresponding errorcode. The ‘mode’ attribute can have the values ‘Off’, ‘On’ and ‘RemoteOnly’ to decide whether the custom error pages handling must be used. ‘RemoteOnly’ will show the custom error pages only when the site isn’t being viewed from the localhost. Within ‘CustomErrors’ you can specify what page to redirect to for a corresponding errorcode, as in the above example.

There are two more interesting attributes that can be used in ‘CustomErrors’: ‘defaultRedirect’ and ‘redirectMode’. ‘DefaultRedirect’ can be used to specify a generic error page to redirect to for an unregistered error code. ‘RedirectMode’ can be used to specify the way of redirecting. Its default value is ‘ResponseRedirect’, which also changes the URL. The other possible value is ‘ResponseRewrite’ which shows custom error pages without changing the URL.

These actions will allow the website to show 404 pages appropriately when there are attempts to go to unknown .aspx pages. But not when there is an attempt to access an unknown folder or other extension. This is because the above rules only apply when the ASP.Net engine has to parse the request. Which isn’t the case for folders. But there is a solution, by adding rules to the ‘httpErrors’ IIS module from within the web.config:

{% highlight xml %}
<system.webServer>

    <httpErrors errorMode ="DetailedLocalOnly">
      <remove statusCode ="404" subStatusCode ="-1"/>
      <error statusCode ="404" path ="/404.aspx" prefixLanguageFilePath="" responseMode="ExecuteURL" />
    </httpErrors>
{% endhighlight %}

‘[HttpErrors](http://www.iis.net/configreference/system.webserver/httperrors)‘ works pretty much the same as ‘customErrors’. The ‘errorMode’ attribute determines when to show the custom error page. ‘DetailedLocalOnly’ will show the custom error pages for requests not coming from the localhost. Within the ‘httpErrors’ tag you’ll have to unregister the default error page first before adding the custom error page. This is done with the ‘remove’ tag with the ‘statusCode’ attribute containing the errorcode. Afterwards you can add the new ‘error’ tag with the corresponding error code, the path to the custom error page, the path for IIS to search a static error page file and the responseMode of the custom error page. These attributes are required to successfully show the custom error page. The ‘responseMode’ attribute can be either ‘File’ if the custom error page is a static file, ‘ExecuteURL’ if it’s a dynamic (.aspx) page or ‘Redirect’ if you want the URL to be changed to the location of the custom error page.

You can also specify default error redirecting in the ‘httpErrors’ tag with the attributes ‘defaultPath’ and ‘defaultResponseMode’. These attributes work the same way as they do for the ‘error’ tag within ‘httpErrors’.