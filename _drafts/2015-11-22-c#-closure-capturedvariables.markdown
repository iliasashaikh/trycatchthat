---
layout: post
title:  "Closures in CSharp"
date:   2015-11-25 21:27:21
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

> In programming languages, closures (also lexical closures or function closures) are a technique for implementing lexically scoped name binding in languages with first-class functions. Operationally, a closure is a record storing a function together with an environment a mapping associating each free variable of the function (variables that are used locally, but defined in an enclosing scope) with the value or storage location to which the name was bound when the closure was created. A closure — unlike a plain function — allows the function to access those captured variables through the closure's reference to them, even when the function is invoked outside their scope.

Wow, that's a mouthful isn't it, let try and dissect the definition by looking at some of the terms, bear with me if these are self evident, but I will expand on them anyways.

- name binding - associating a symbol with an entity such as a variable to its value or address, or a symbol to an operation. Consider the following
    {% highlight c# %}
        // variable 'i' is bound to 100
        int i = 100; 
        
        // considering AA to be a reference type, 
        // variable 'a' is bound to the start address of the location on the heap where the bytes for the instance of 'AA' are stored
        AA a = new AA(); 
        
        // variable 'act' is bound to the anonymous function 
        Action act = ()={ Console.WriteLine ("hello")}; 
    {% endhighlight %}

- lexical scope - this is a region of statements in the your code where a variable is visible. A variable is visible in a statement if it can be referenced from the statement.
- 
- first class functions - a programming language is said to have first class functions when functions can be treated as any other data. In c# first class functions are supported using anonymous methods and lambdas.
- 
- free variable

In the above example, the anonymous function referenced by the delegate is a closure, since this function now 'closes' over the variable 'i' in the scope where it is declared. As a concept, it is more functional than object oriented. In can



'Closure'

In this example we can say that the variable 'i' has been captured by the anonymous function.

Outer variable

Captured outre variable

Now rather than outputting the variable to the console immediately, lets do something else and capture some variables 

{% highlight c# %}

// Snippet #2
public void foo()
{
  Action<int> a = (i) => Console.Write($"{i} ");
  List<Action> actions = new List<System.Action>();
  
  // capture the loop variable i by the action closure we defined earlier
  for (int i = 0; i < 10; i++)
    actions.Add(()=>a(i));
  
  foreach (var action in actions)
    action();
}

{% endhighlight %}

This time round, the output turns out to be a bit unexpected 
    10 10 10 10 10 10 10 10 10 10 

Lets now slightly alter the above code to use a *__foreach__* instead of *__for__*    

{% highlight  c# %}
// Snippet #3
public void foo()
{
  Action<int> a = (i) => Console.Write($"{i} ");
  List<Action> actions = new List<System.Action>();
  
  foreach (int i in Enumerable.Range(0,10))
    actions.Add(()=>a(i));
  
  foreach (var action in actions)
    action();
}
{% endhighlight %}

And now the output is different again 

    0 1 2 3 4 5 6 7 8 9

    