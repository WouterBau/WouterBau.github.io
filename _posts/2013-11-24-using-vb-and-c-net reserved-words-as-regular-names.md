---
layout: post
title:  "Using VB.Net and C#.Net reserved words as regular names"
date:   2013-11-24
description: You can take away the meaning of reserved words to use them any way you like. But I don’t advise doing so unless it’s really appropriate to do so. For example, as an entry of an enumeration.
tags:
 - VB
 - C#
 - .NET
share: true
---
I don’t advise to use this unless you really have to. When using .Net reserved words for other purposes, it can become confusing for you or your collaborators. So use this only when it’s necessary or if it might actually help. I’ve come across a good example of the second option while creating an enumeration.

>VB
{:.filename}
{% highlight vb %}
Public Enum LogSeverity
     Info = 0
     Warning = 1
     [Error] = 2
End Enum
{% endhighlight %}

>C#
{:.filename}
{% highlight cs %}
public enum LogSeverity
{
     Info = 0,
     Warning = 1,
     @Error = 2
}
{% endhighlight %}

So by using ‘[‘ and ‘]’ in VB.Net and ‘@’ in C#.Net, you can take away the meaning of the reserved word and use it as you like. But as stated above, avoid this as much as possible unless it’s appropriate like in the examples above.