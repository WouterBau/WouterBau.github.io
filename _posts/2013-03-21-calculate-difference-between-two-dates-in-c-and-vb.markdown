---
layout: post
title:  "Calculate difference between two dates in C# and VB"
date:   2013-03-21
description: Calculating the difference between dates couldn’t have been easier as in C# or VB. I’ll show you how in both languages and how to calculate the difference in years and months and more.
tags:
 - VB
 - C#
---
In at least one of your projects, you’ll have to calculate the difference between the two dates. And if you enjoyed programming in C# or VB already, you’ll enjoy even more what tools they have provided to do so.

Calculating the difference between dates in C#:

{% highlight csharp %}
DateTime dt1 = DateTime.Now;
DateTime dt2 = new DateTime(2013, 3, 16);

//The difference between two DateTime objects generates a TimeSpan object
Console.WriteLine("One line solution for days: " + (dt1 - dt2).Days);

//TimeSpan object containing the difference in time between the provided two dates
TimeSpan dtspan = dt1 - dt2;

//You can return the timedifference in other formats as well
Console.WriteLine("Hours: " + dtspan.Hours);
Console.WriteLine("Minutes: " + dtspan.Minutes);
Console.WriteLine("Seconds: " + dtspan.Seconds);
Console.WriteLine("Milliseconds: " + dtspan.Milliseconds);

//You can also return the difference between the dates in whole and fractional days.
Console.WriteLine("Total Days: " + dtspan.TotalDays);
{% endhighlight %}

Calculating the difference between dates in VB:

{% highlight vb %}
Dim dt1 As DateTime = DateTime.Now
Dim dt2 As DateTime = New DateTime(2013, 3, 16)

'The difference between two DateTime objects generates a TimeSpan object
Console.WriteLine("One line solution for days: " & (dt1 - dt2).Days)

'TimeSpan object containing the difference in time between the provided two dates
Dim dtspan As TimeSpan = dt1 - dt2

'You can return the timedifference in other formats as well
Console.WriteLine("Hours: " & dtspan.Hours)
Console.WriteLine("Minutes: " & dtspan.Minutes)
Console.WriteLine("Seconds: " & dtspan.Seconds)
Console.WriteLine("Milliseconds: " & dtspan.Milliseconds)

'You can also return the difference between the dates in whole and fractional days.
Console.WriteLine("Total Days: " & dtspan.TotalDays)
{% endhighlight %}

So both languages work the same way with the TimeSpan object. But what you notice is the lack of calculating the difference in, for example, years or months. There is no standard solution for this in C#. But VB (And SQL) however has the ‘DateDiff‘ function which allows to calculate the difference in more ways than the above examples:

{% highlight vb %}
Dim dt1 As DateTime = DateTime.Now
Dim dt2 As DateTime = New DateTime(2013, 3, 16)

'Using DateDiff to calculate the difference between two dates
Console.WriteLine("Years: " & DateDiff(DateInterval.Year, dt2, dt1))
Console.WriteLine("Quarters of a year: " & DateDiff(DateInterval.Quarter, dt2, dt1))
Console.WriteLine("Months: " & DateDiff(DateInterval.Month, dt2, dt1))
Console.WriteLine("Weeks of the year: " & DateDiff(DateInterval.WeekOfYear, dt2, dt1))
Console.WriteLine("Days: " & DateDiff(DateInterval.Day, dt2, dt1))
Console.WriteLine("Hours: " & DateDiff(DateInterval.Hour, dt2, dt1))
Console.WriteLine("Minutes: " & DateDiff(DateInterval.Minute, dt2, dt1))
Console.WriteLine("Seconds: " & DateDiff(DateInterval.Second, dt2, dt1))
{% endhighlight %}

This looks quite nice. But I’ll still suggest trying to use the TimeSpan objects as much as possible. Because after testing these two, I’ve noticed a difference between returned values from these functions. This will probably have to do with how the calculation is done for the time-difference when nor hours, minutes or seconds are given:

![Date Difference](/assets/images/datedifference.png "datedifference")

If you still want to use the ‘DateDiff’ function in C#, you can always reference the Microsoft.VisualBasic dll in your project and use the function from it. Or you can use the ‘DateDiff’ functions used by Linq when generating SQL-queries. These generate the same results as the VB ‘DateDiff’ functions, but offers other formats:

{% highlight csharp %}
DateTime dt1 = DateTime.Now;
DateTime dt2 = new DateTime(2013, 3, 16);

//Using DateDiff functions from Linq
Console.WriteLine("Years: " + System.Data.Linq.SqlClient.SqlMethods.DateDiffYear(dt2, dt1));
Console.WriteLine("Months: " + System.Data.Linq.SqlClient.SqlMethods.DateDiffMonth(dt2, dt1));
Console.WriteLine("Days: " + System.Data.Linq.SqlClient.SqlMethods.DateDiffDay(dt2, dt1));
Console.WriteLine("Hours: " + System.Data.Linq.SqlClient.SqlMethods.DateDiffHour(dt2, dt1));
Console.WriteLine("Minutes: " + System.Data.Linq.SqlClient.SqlMethods.DateDiffMinute(dt2, dt1));
Console.WriteLine("Seconds: " + System.Data.Linq.SqlClient.SqlMethods.DateDiffSecond(dt2, dt1));
Console.WriteLine("Milliseconds: " + System.Data.Linq.SqlClient.SqlMethods.DateDiffMillisecond(dt2, dt1));
{% endhighlight %}