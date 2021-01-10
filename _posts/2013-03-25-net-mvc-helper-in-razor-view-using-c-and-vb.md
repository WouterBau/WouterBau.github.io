---
layout: post
title:  ".Net MVC @helper in Razor view using C# and VB"
date:   2013-03-25
description: Creating reusable pieces of HTML with dynamic data for .Net MVC Razor made quick and easy with helpers.
tags:
 - ASP.NET MVC
 - VB
 - C#
---
Since ASP.Net MVC3, the Razor View Engine has been introduced to generate the HTML views. When you have started making MVC applications, you’ve definitely used this already and are familiar with creating logic and accessing the @Model in .vbhtml or .cshtml files. But have you heard of @Helper to create reusable pieces of View?

# Using helpers
Sometimes there will be bits of html or strings that would return in different pages which aren’t complex enough to dedicate a whole partial view for. For example, the translation of a word or a class that has to be filled in on a certain occasion. For these tiny bits of returning html, you can create helpers within your view.

A helper can easily be defined and used within your view like so:

>Razor
{:.filename}
{% highlight razor %}
@helper GiveAName(string text){
	if(String.IsNullOrWhitespace(text)){
		<h2>Default name</h2>
	}else{
		<h2>@text</h2>
	}
}

@GiveAName("Has a Name")
<p>First example having a name</p>
@GiveAName("")
<p>This second example receives a default name from the helper</p>
{% endhighlight %}

# Creating helpers to use on multiple views
This is nice and easy to create and use. But now you want to use the same little helper on all your views. However, it is bad practice to paste the same piece of code over and over again. So you want to define your helpers where you can call them up in every view you create.

![MVC Helpers](/assets/images/mvchelpers.png "MVC Helpers")

To do this, you’ll have to create a .cshtml (or .vbhtml) file in the “App_Code” directory of your project. Here you will be able to create a library of helpers which you can reuse throughout your whole project.

You then just simple write the helper as you did in the regular view:

>C#
{:.filename}
{% highlight cs %}
@helper GiveAName(string text)
{
     if(String.IsNullOrWhitespace(text))
     {
          <h2>Default name</h2>
     }
     else
     {
          <h2>@text</h2>
     }
}

@helper GetClassName(string text)
{
     if(String.IsNullOrWhitespace(text))
     {
          @: DefaultClass
     }
     else
     {
          @: TextClass
     }
}
{% endhighlight %}

Or when you are using VB.Net:

>VB
{:.filename}
{% highlight vb %}
@helper GiveAName(ByVal text As String)
     @If String.IsNullOrWhitespace(text) Then
          @<h2>Default name</h2>
     @Else
          @<h2>@text</h2>
     @End If
End Helper

@helper GetClassName(ByVal text As String){
     @If String.IsNullOrWhitespace(text) Then
          @: DefaultClass
     @Else
          @: TextClass
     @End If
End Helper
{% endhighlight %}

As you can see, VB.Net requires the ‘@’ symbols much more than the C# version of Razor View. So C# improves the readability of your HTML code, although VB.Net reads more like a piece of program flow.

The helpers you have created will be available in the ‘Helpers’-class (depends on how you named the file) after compilation. Visual Studio will also provide you with intellisense when creating a call to the requested helper.

>Razor
{:.filename}
{% highlight razor %}
@Helpers.GiveAName("Has a Name")
<p class="@Helpers.GetClassName("Classname")">First example having a name</p>
@Helpers.GiveAName("")
<p class="@Helpers.GetClassName("")">This second example receives a default name from the helper</p>
{% endhighlight %}

# Calling a helper within logic in a view
You can also use helpers within your codeblocks in a Razor View .cshtml file. It’s as easy as using them within the html tags of the view. But be warned! The helpers return an object of the HelperResult class! So if you want to use these in code, don’t forget to call a .ToString() function on them to get the raw HTML out of it.

>Razor
{:.filename}
{% highlight razor %}
@if(Helpers.GetClassName(Model.Title).ToString().Equals("DefaultClass")){
     <p>Only shows when default class name is returned</p>
}
{% endhighlight %}