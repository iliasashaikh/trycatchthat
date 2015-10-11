---
layout: post
title:  "Under construction"
date:   2015-10-11 22:43:15
categories: none
---

Only just started this website. I am working on building some content to publish. In the meantime, read the following code and think what the output would be 

{% highlight c# %}

public static void Main()
{
  var l = new List<int>{1,2,3};
  Bar(l);
  Console.WriteLine (l.Count);
  
  Bar(ref l);
  Console.WriteLine (l.Count);
}
 
static void Bar(List<int> list)
{
  list = null;
}
 
static void Bar(ref List<int> list)
{
  list = null;
}

{% endhighlight %}

![Under construction](/assets/underconstruction.jpg)