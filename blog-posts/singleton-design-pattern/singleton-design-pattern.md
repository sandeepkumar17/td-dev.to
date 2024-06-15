---
published: true
title: 'Singleton Design Pattern and different ways to implement it in C#'
cover_image: 'https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/assets/blog-cover/design-pattern.png'
description: 'Example of Singleton Design Pattern using C#'
tags: csharp, dotnet, programming, architecture
series:
canonical_url:
---

The singleton pattern is one of the best-known patterns in software engineering. In a singleton pattern, we ensure that the class has only one instance and we provide a simple and global point of access to this object. This is useful when we require exactly one object of a class to perform our operations.

Singleton classes don't allow any parameters to be specified when creating the instance because a second request for an instance but with a different parameter could be problematic.

> In this article, I'll provide you with a real-time example, a Comparison of Singleton with Static class, a UML class diagram of Singleton, and then the implementation of Singleton in various ways.

### Real-time example:
The practical implementation of the Singleton design pattern can be seen in various applications used by us. Take the example of Microsoft Word, there is only one instance of the word application active at a time even though the user opened multiple documents at a time. All the requests for different Word documents are handled by a single instance.

### Singleton versus Static Class:
Singletons and Static classes both provide the sharing of redundant objects in memory, but they differ in usage and implementation. Mentioned below are some points one should consider before making a decision:
- Singletons preserve the conventional class approach and don't require that you use the static keyword everywhere, while static class requires all members should be static.
- Singleton may be more demanding to implement at first, but will greatly simplify the architecture of your application as compared to static class.
- Singleton requires a single private and parameter-less constructor, while a static class can only have a static constructor.
- Unlike static classes, one can use a singleton as a parameter to methods, or objects.
- In the case of the Singleton class, there should be at most one object through which we can access the class members, while in the case of the Static class we needn't any object to access class members.
- Singleton object is created lazily, i.e. the instance isn't created until it is first needed. On the other hand, the static class is loaded into memory as soon as the application is initialized.

### UML class diagram
The UML class diagram for the Singleton pattern is given below.

![Singleton UML](./assets/singleton_UML.png 'Singleton UML')

### Implementation:
In the case of the Singleton pattern, there is a single private and parameter-less constructor. This prevents other classes from instantiating it. It also prevents subclassing because if a singleton can be subclassed and if each of those subclasses can create an instance that means a violation in the pattern.
Other than that there is a static variable (or we can have a method as well) that holds a reference to the single created instance. And on top of that a public static means of getting the reference to the single created instance.

Here we'll see different ways of implementing the singleton pattern in C#.

**A simple version of Singleton:**

It is not thread-safe, i.e. two different threads may create two instances.

{% gist https://gist.github.com/sandeepkumar17/bc9cba456f3017d75eb1948a2cd5bf98 %}

**Singleton with simple thread safety:**

In this implementation, the lock is added to make Singleton thread-safe. In this case, the thread takes out a lock and then checks if the instance already exists or not before creating the new instance. This ensures that only one thread will create an instance as only one thread can be in that part of the code at a time - by the time another thread enters, the first thread will have created the instance. However there is a performance overhead as a lock is acquired every time the instance is requested.

{% gist https://gist.github.com/sandeepkumar17/a79d2a10a31b88b8c97c6b0bfce9c04a %}

**Singleton with thread-safety using double-checks locking:**

This implementation attempts to be thread-safe along with avoiding taking a lock every time.

{% gist https://gist.github.com/sandeepkumar17/cfa297e215c2f5d4dbcee0017a3a5273 %}

**Singleton with thread safety without using locks:**

This is another version of thread-safe Singleton, but without using locks. This implementation is faster than adding extra checking as in the above example. But this implementation is not as lazy as the other implementations (as we are using a static constructor).

{% gist https://gist.github.com/sandeepkumar17/80d0a038de5c17c02ca4aa2202270d1d %}

**Singleton with full lazy instantiation:**

To fix the problem mentioned in the above example, we can modify the code and create a private class inside the Singleton class and that'll allow full lazy instantiation along with all the performance benefits.

{% gist https://gist.github.com/sandeepkumar17/96396e49d0e8a7d7c8e40a03fa05f01e %}

**Singleton with .Net Lazy&#60;T>:**
  
If you're implementing Singleton in .NET 4.0 (or higher versions), you can use the [System.Lazy&#60;T>](http://msdn.microsoft.com/en-us/library/dd642331%28v=vs.110%29.aspx) type to make the lazy instantiation. You can easily write the code with the help of lambda expression and you just need to pass a delegate to the constructor which calls the constructor of the Singleton class. It's easy to write and understand and also performs well. You can also use the [IsValueCreated](http://msdn.microsoft.com/en-us/library/dd642334.aspx) property (in the .Net 4.5 frameworks) to check whether or not the instance has been created yet.

{% gist https://gist.github.com/sandeepkumar17/f4d915d80e383995533bda6fbbeac939 %}

<div style="text-align:center">
  <img src="https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/assets/summary.png" />
</div>
 
:bookmark_tabs: Singletons allow you to reuse code and control object states much more easily.
  
:heavy_check_mark: We should go with Singleton only when exactly one instance is required.
  
:heavy_check_mark: There are various ways of implementing the singleton pattern in C#. One has to decide on performance along with laziness while creating the Singleton.
  
:heavy_check_mark: Like if someone wants to go with thread safety, then can go with either option 2 or 3(with double check).
  
:heavy_check_mark: But if you choose performance over laziness and don't want to use locking then probably you'll go with the fourth option.

:heavy_check_mark: However, if you want to go with lazy instantiation and don't want to use locks as well, then options 5 or 6 are good to go. But option 6 can be used in .Net 4.0 or higher versions.

## NOTE:
If you have any comments or suggestions, please leave them behind in the comments section below.

---
_Originally published at [http://diary-techies.blogspot.com](https://diary-techies.blogspot.com/2014/04/singleton-design-pattern-in-c.html) on April 23, 2014._
