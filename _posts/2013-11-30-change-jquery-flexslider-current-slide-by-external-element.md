---
layout: post
title:  "Change JQuery Flexslider current slide by external element"
date:   2013-11-30
description: Two alternatives to changing the current slide of a JQuery Flexslider from a HTML element outside of the slider.
---
The JQuery [Flexslider](http://www.woothemes.com/flexslider/) is a very handy Javascript plugin to create sliders of various form of slides. By creating a simple list and performing the appropriate short piece of code, you have an easy and swipe-enabled slider out of the box (After adding the piece of code from the Flexslider website):

{% highlight xml %}
<div class="js-carousel">
    <ul class="slides">
        <li>
            <article>
            <!-- Content -->
            </article>
        </li>
        <li>
            <article>
            <!-- Content -->
            </article>
        </li>
    </ul>
</div>
{% endhighlight %}

{% highlight js %}
$(".js-carousel").flexslider({
    animation: "slide",
    slideshow: !1
});
{% endhighlight %}

But sometimes you’d like to change the current slide of the slider from outside of it. When clicking on a shortcut to a certain slide containing more information, for example.

{% highlight xml %}
<a href="#" rel="2" class="slider-box">
    <!-- Content -->
</a>
{% endhighlight %}

I have found solutions that make use of the ‘start’-property when creating the slider where the click of the button’s event is registered to change to a slide. This is done then because the context of the Flexslider is available. But I’ve found it doesn’t always do the trick and it’s also a bit confusing.

{% highlight js %}
$(".js-carousel").flexslider({
    animation: "slide",
    slideshow: !1,
    start: function(slider) {
        $('.slider-box').click(function(event){
            event.preventDefault();
            var pos = parseInt(this.rel, 10);
            if (pos !== slider.currentSlide) {
                slider.flexAnimate(pos, true);
            }
        });
    }
});
{% endhighlight %}

However, this doesn’t seem to work every time (or at least not in the project I was using it in). After some tinkering and searching and trying out solutions, I think I’ve found a more straight forward and simpler way of tackling this problem. When the click event is happening for the link, you can just as easily request the context of the slider from the element.

{% highlight js %}
$('.slider-box').bind('click', function () {
    var pos = parseInt(this.rel, 10);
    if (pos !== $(".js-carousel").data('flexslider').currentSlide) {
        $(".js-carousel").data('flexslider').flexAnimate(pos, true);
    }
});
{% endhighlight %}

Much easier to read and a more logic place of action to my feelings.