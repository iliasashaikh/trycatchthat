---
layout: post
title:  "Closures in c# - Loops"
date:   2016-02-29 06:55:55
categories: csharp fundamentals
---

In the [last post]({% post_url 2015-12-27-csharp-closure-thebasics %}) I looked at the fundamentals of closures. In this post I will look at some of the things to watch for when using closures. 

One of the key takeaway from the last post was the fact that closures captures variables and not values. This has at least one counter intuitive affect on loops.

Consider the following code 

{% highlight c# %}
void foo()
{
  Action[] acts = new Action[10];
  for (int i = 0; i < 10; i++)
  {
    acts[i] = ()=>Console.Write(i + " ");
  }
  
  foreach (var a in acts)
  {
    a();
  }
}
{% endhighlight %}

On running this the output is 
    
    10 10 10 10 10 10 10 10 10 10 

To see what's happening here we can again try and decompose this snippet to what the compiler would do. The code below will not compile because of the use of the non existent keyword __*'ref'*__ in @AnoymousClass001. This is to illustrate the fact that it is the reference that is being used and not the value. *Otherwise int being a value type it would not have been possible to illustrate my point*

{% highlight c# %}
void foo()
{
  Action[] acts = new Action[10];
  for (int i = 0; i < 10; i++)
  {
    var r = new AnonymousClass001();
    r.i = ref i;
    acts[i] = r.AnonymousMethod001;
  }
  
  foreach (var a in acts)
  {
    a();
  }
}

class @AnonymousClass001
{
  internal ref int i; // the ref keyword does not exist in c# and is used only for illustrative purposes

  internal void @AnonymousMethod001()
  {
    Console.WriteLine(i + " ");
  }
}
{% endhighlight %}

Hopefully, that explains why we get all 10s instead of numbers in sequence as we really wanted. To get the code to do as we want, we can change our loop block to declare a variable inside the loop block, like so - 

{% highlight c# %}
void foo()
{
  Action[] acts = new Action[10];
  for (int i = 0; i < 10; i++)
  {
    int j  = i; // introduce a local variable inside the loop block
    acts[i] = ()=>Console.Write(j + " ");
  }
  
  foreach (var a in acts)
  {
    a();
  }
}
{% endhighlight %}

This will give us the output as - 

    0 1 2 3 4 5 6 7 8 9 

Hopefully the reason why this works is fairly intuitive by now. The loop variable __*'i'*__ is shared across all the runs of the loop, while a instance of the variable __*'j'*__ is created every time the loop block is run