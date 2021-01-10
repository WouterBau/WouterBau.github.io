---
layout: post
title:  "Serving other content in ASP.Net Response"
date:   2016-06-06
description: You want to generate a file or serve a document other than HTML out of ASP.Net and want to give it all the correct headers and filename in the response? It requires only a few lines to get it done.
---
It’s not always HTML you’d want to serve to the visitor when they open an URL. In certain occasion the URL can be a link to a dynamically generated PDF file (like an receipt for an online order) or another file. To ensure the client opens or downloads this file correctly, you have to set the contenttype and headers for the response. Of course you need to stream the content to the response too.  When you get your content in a binary array, you can achieve your wish with these few lines.

{% highlight cs %}
byte[] data = GetDataToServe();
Response.BinaryWrite(data);
Response.ContentType = "application/pdf";
Response.AddHeader("Content-Disposition", "inline; filename=test.pdf");
{% endhighlight %}

The [HttpResponse](https://msdn.microsoft.com/en-us/library/system.web.httpresponse(v=vs.110).aspx) class ‘Response’ of the current context will first write the binary array to it’s outputstream. It then sets the [content type](http://www.iana.org/assignments/media-types/media-types.xhtml) of the response you’re going to return. Here you have to make sure it’s the contenttype that matches your output, otherwise the client will fail to show it correctly. To top it all off, we add a HTTP header ‘[Content-Disposition](http://www.ietf.org/rfc/rfc1806.txt)‘ which will communicate the filename of the output for when the client would want to save the file.

Do mind, it might be useful to call the Clear method of the ‘Response’ class first. To ensure no other data will be in the outputstream once you start feeding it with that of the file. This could otherwise follow up into a corrupt file you’re trying to send.

{% highlight cs %}
Response.Clear();
{% endhighlight %}