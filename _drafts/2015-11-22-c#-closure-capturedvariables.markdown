---
layout: post
title:  "Closures in CSharp the basics"
date:   2015-12-27 11:08:04
categories: c# fundamentals
---
*Definition* 

Consider this code snippet, 

##### Basic closure {#listing1}
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


On running this piece of code in a Console application you see -

    100

On first glance this is fairly intuitive and obviously the output we would expect, but hang on, how can function *bar()* get access to the local variable of *foo()*. It is, as you might image, a piece of compiler jiggery-pokery that makes this possible, but more on that later. We have just seen in this [listing](#listing1) a 'closure' in action. The wikipedia definition of closure is 

> In programming languages, closures (also lexical closures or function closures) are a technique for implementing lexically scoped name binding in languages with first-class functions. Operationally, a closure is a record storing a function together with an environment : a mapping associating each free variable of the function (variables that are used locally, but defined in an enclosing scope) with the value or storage location to which the name was bound when the closure was created. A closure — unlike a plain function — allows the function to access those captured variables through the closure's reference to them, even when the function is invoked outside their scope.

Wow, that's a mouthful isn't it, let try and dissect the definition by looking at some of the terms, bear with me if these are self evident, but I will expand on them anyways.

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

- lexical scope - this is a region of statements in the your code where a variable is visible. A variable is visible in a statement if it can be referenced from the statement.

- first class functions - a programming language is said to have first class functions when functions can be treated as any other data. In c# first class functions are supported using anonymous methods and lambdas.

- free variable - a free variable, in simple terms, is a variable that is not bound to a value, i.e. its not a function parameter and its not defined in the same function.

Now that we have set some background, lets try and understand the definition of closure in the context of our original code. 
Consider the lambda *() => Console.Write($"{i} ");*, here the variable 'i' is free, since its not a parameter to the lambda or defined internally. Instead, it gets its value from the environment when the lambda was defined. A closure is hence, a record that stores a function and all the variables/ data that it uses from the environment. In the c# world, we have a handy way of enclosing functions and variables - called a **class**. And indeed, thats how closures are implemented. So, the code in [snippet #1](#listing1) translates into something like this - please note the 'something', the actual code generated will probably look a lot different.

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

