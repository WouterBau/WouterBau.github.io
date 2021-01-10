---
layout: post
title:  "Blocking right-click contextmenu on images with Javascript and within Fancybox"
date:   2014-03-30
description: Blocking the right-click contextmenu isn’t advised nor user-friendly. But when the customer has expensive pictures, they want to make it hard to download them.
---
Great looking sites have great looking pictures, most of the times. But some clients invested a lot in creating those pictures and don’t want those pictures to be used by others for different purposes. I understand that, I do. But everything is public on the internet, you can’t go fully blocking access to anything. Unless you force visitors to log in and all such. But those systems aren’t fully safe neither. (Example: I could pull images of private student profiles after searching their unique identifier from an address book in the school’s online environment when I was still a student as well)

So, the customer will sometimes ask you to block the possibility to right-click on an image so it can’t be saved as easily. You should then try to explain to the customer that it isn’t user-friendly. All visitors expect to be able to right-click anywhere they want within their screen. You should also explain that there is no sure way to make that work neither, because the right-click contextmenu is at clientside, so you do not have full control over it. The only way to block the right-click contextmenu on a site is with Javascript. So browsers with Javascript disabled will still be able to save images. And visitors will still be able to take a screenshot of the site to rip an image.

When you have warned your customer of all these facts and they still decide to have the right-click blocked, you can make sure the blocking only happens on images. This snippet of Javascript with JQuery will do the trick when placed within the document ready statement:

{% highlight js %}
$("img").bind("contextmenu", function (e) {
     return false;
});
{% endhighlight %}

But, fancy sites with fancy pictures also have fancy photogalleries. I have used [Fancybox](http://fancybox.net/) most of the time to be able to pop-up the larger version of an image and to navigate to the next of previous image within that pop-up. The piece of code below shows how you can configure Fancybox to block the contextmenu as well. It’s pretty much similar to the code above.

{% highlight js %}
$(".js-popup a, a.js-popup").fancybox({
     padding: 0,
     nextEffect: "fade",
     prevEffect: "fade",
     helpers: { overlay: { locked: !1 } },
     afterLoad: function () {
          $("img").bind("contextmenu", function (e) {
               return false;
          });
     },
     onUpdate: function () {
          $("img").bind("contextmenu", function (e) {
               return false;
          });
     }
});
{% endhighlight %}

This piece of code makes sure the context-menu is blocked after the pop-up has loaded, but also when the visitor navigates to the next or previous image.