---
layout: post
title:  "Get name of month or day in C# and VB.Net"
date:   2014-04-06
description: The DateTime class is very useful. But sometimes you just want to print out the name of the month or day you have the integer value of, without the hassle of creating a whole DateTime object.
---
The [DateTime](http://msdn.microsoft.com/en-us/library/system.datetime(v=vs.110).aspx) structure in .Net is very useful and powerful for many purposes. It’s one of those basic classes you get in every programming language to be able to do whatever you need with dates. For example, print out a string of the current date in a certain format by the current culture standards:

C#:

{% highlight csharp %}
Console.WriteLine(DateTime.Today.ToString("dd MMMM yyyy"));
 //Writes '06 april 2014' in nl-BE culture
{% endhighlight %}

VB:

{% highlight vb %}
Console.WriteLine(DateTime.Today.ToString("dd MMMM yyyy"))
'Returns the same as the C# code, so it's practically the same
{% endhighlight %}

The whole list of format options has been summed up at the ‘[Custom Date and Time Format Strings](http://msdn.microsoft.com/en-us/library/8kb3ddd4(v=vs.110).aspx)‘ msdn page.

But sometimes you only have need for the name of the month or the day in the current culture and you have the integer value for these. You can create a whole DateTime object which situates in the given month. But I dislike doing that step because I don’t need it. I don’t require to have the rest of the date properties nor do I need to do calculations. It’s overhead in my eyes. After a bit of searching, I came to these alternatives:

{% highlight csharp %}
Console.WriteLine(CultureInfo.CurrentCulture.DateTimeFormat.GetMonth(4));
//Return 'april', the name of the month in the current culture
Console.WriteLine(CultureInfo.CurrentCulture.DateTimeFormat.GetAbbreviatedMonthName(4));
//Return 'apr', the abbreviation of the month in the current culture

Console.WriteLine(CultureInfo.CurrentCulture.DateTimeFormat.GetDay(DayOfWeek.Sunday));
//Return 'zondag', the weekday in the current culture
Console.WriteLine(CultureInfo.CurrentCulture.DateTimeFormat.GetAbbreviatedDayName(DayOfWeek.Sunday));
//Return 'zo', the abbreviation of the weekday in the current culture
{% endhighlight %}

This works exactly the same in VB.Net as well. ‘CultureInfo.CurrentCulture.DateTimeFormat’ returns a [DateTimeFormatInfo](http://msdn.microsoft.com/en-us/library/system.globalization.datetimeformatinfo(v=vs.110).aspx) object which you can use to get localized format information for the current culture.

In VB.Net you also have the [MonthName](http://msdn.microsoft.com/en-us/library/zxbsw165(v=vs.90).aspx) and [WeekdayName](http://msdn.microsoft.com/en-us/library/t8dc1aee(v=vs.90).aspx) global functions, which do exactly the same as above. But even a lot shorter:

{% highlight vb %}
Console.WriteLine(MonthName(4))
'Writes "april"
Console.WriteLine(MonthName(4, True))
'Writes "apr"

Console.WriteLine(WeekdayName(1))
'Writes "zondag"
Console.WriteLine(WeekdayName(1, True))
'Writes "zo"
{% endhighlight %}