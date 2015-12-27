---
layout: post
title:  "Closures in CSharp - The Basics"
date:   2015-12-27 11:08:04
categories: c# fundamentals
---

Consider this code snippet, 

{% highlight c# %}
public void foo()
{
  int i = 100;
  Action a = () => Console.Write($"{i} ");
  bar(a);
}

public void bar(Action a)
{
  a();
}

{% endhighlight %}


On running this piece of code in a console application you see -

    100

On first glance this is fairly intuitive and obviously the output we would expect, but hang on, how can function '__*bar()*__' have access to the local 'i' variable of __*foo()*__. 

We have just seen a 'closure' in action.

It is, as you might imagine, a piece of compiler trickery that makes this possible, but more on that later. 

__So, what exactly is a 'closure'?__ 

According to wikipedia - 

> In programming languages, closures (also lexical closures or function closures) are a technique for implementing lexically scoped name binding in languages with first-class functions. Operationally, a closure is a record storing a function together with an environment : a mapping associating each free variable of the function (variables that are used locally, but defined in an enclosing scope) with the value or storage location to which the name was bound when the closure was created. A closure — unlike a plain function — allows the function to access those captured variables through the closure's reference to them, even when the function is invoked outside their scope.

Wow, that's a mouthful! Its not however completely inscrutable, before we try and dissect this statement, let me define some of the terms used, bear with me if these are self evident, but I will expand on them anyways.

- lexical scope - this is a region of statements in the your code where a variable is visible. A variable is visible in a statement if it can be referenced from the statement.

- name binding - associating a symbol with an entity such as a variable to its value or address, or a symbol to an operation. Consider the following
    {% highlight c# %}
        // variable 'i' is bound to 100
        int i = 100; 
        
        // considering AA to be a reference type, 
        // variable 'a' is bound to the start address of the location on the heap where the bytes for the instance of 'AA' are stored
        AA a = new AA(); 
        
        // variable 'act' is bound to the anonymous function 
        Action act = ()=> { Console.WriteLine ("hello")}; 
    {% endhighlight %}


- first class functions - a programming language is said to have first class functions when functions can be treated as any other data. In c# first class functions are supported using anonymous methods and lambdas.

- free variable - a free variable, in simple terms, is a variable that is not bound to a value, i.e. its not a function parameter and its not defined in the same function.

Now that we have set some background, lets try and understand the definition of closure in the context of our original code. 
Consider the lambda *() => Console.Write($"{i} ");*, here the variable 'i' is free, since its not a parameter to the lambda or defined internally. Instead, it gets its value from the environment when the lambda was defined. A closure is hence, a record that stores a function and all the variables/ data that it requires from the environment. In the c#/ .net world, we have a handy way of enclosing functions and variables - called a **class**. And indeed, thats how closures are implemented. So, the code in [snippet #1](#listing1) translates into something like this - please note that actual code generated probably looks a lot different.

{%highlight c#%}
public void foo()
{
  int i = 100;
  var r = new @AnonymousClass001();
  r.i = i;
  Action a = r.AnonymousMethod001;
  bar(a);
}

public void bar(Action a)
{
  a();
}

class @AnonymousClass001
{
  internal int i;
  internal void @AnonymousMethod001()
  {
    Console.Write($"{this.i}");
  }
}
{%endhighlight%}

If you think about it, this is possibly the way you would have implemented closures if you'd been asked to. 

Another thing to note from the implementation is that closures capture the 'variable' and not the 'value'. Lets update our original code slightly to see what the implication of this is. 
In the following code we increment the value of i __*after*__ the lambda has been defined.

{% highlight c# %}
public void foo()
{
  int i = 100;
  Action a = () => Console.Write($"{i} ");
  i++; // increment i after the lambda has been defined
  bar(a);
}

public void bar(Action a)
{
  a();
}

{% endhighlight %}

Since, the closure captures the variable 'i', rather than the value '100', this outputs 

    101