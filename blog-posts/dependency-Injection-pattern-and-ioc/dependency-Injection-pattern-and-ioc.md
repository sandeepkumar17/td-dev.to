---
published: false
title: 'Dependency Injection pattern and Inversion of Control with C#'
cover_image: 'https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/blog-posts/dependency-Injection-pattern-and-ioc/assets/dependency_injection.png'
description: 'Under about Dependency Injection pattern and Inversion of Control and review its implementation in C#'
tags: csharp, architecture, programming, discuss
series:
canonical_url:
---

## Understand the Object Dependency and Coupling:
When an object needs another object for required task, then the first object is dependent on the second.

**For example** three objects, namely, X, Y, and Z. If object X is coupled to object Y, and Y is in turn coupled to Z, then object X is effectively coupled to object Z, i.e. it is dependent on Z. This behavior is known as transitivity.

## There are two ways to couple the objects:
- **Tight Coupling:** In this, the objects are not independently reusable and hence are difficult to use effectively in unit test scenarios.
- **Loose Coupling:** In this scenario an object is loosely coupled with another object, and you can change the coupling with ease and can be used easily in unit test scenarios.

## Dependency Injection (DI):
The basic idea behind the Dependency Injection is to have a separate object and "Design must be loosely coupled". Dependency injection eliminates tight coupling between objects to make both the objects and applications that use them more flexible.

**Key points about Dependency Injection (DI):**
- It is also known as Inversion of Control (IOC) container.
- It can help you design your applications so that the architecture links the components rather than the components linking themselves.
- IOC provides a great way to reduce tight coupling between software components.
- Basic idea behind DI is design must be loosely coupled, loosely coupled means that objects should have only those dependencies which are required to complete their task and number of dependencies must be fewer.
- Loose coupling offers us greater reusability and testability.
- With the help of DI we can achieve code maintainability.
- Object dependencies should be an interface not on "concrete" objects.
- It enables us to manage future changes and other complexity in our application.

**There are three basic ways to implement the Dependency Injection:**
- Construction Injection
- Setter Injection
- Interface Injection.

## Constructor Injection
Constructor injection uses parameters to inject dependencies. It is most common form of Dependency Injection. The basic idea behind this DI technique is passing an object's dependencies to its constructor. In case of constructor-injection the object has no defaults constructor and we need to pass specified values at the time of creation to instantiate the object.

**Advantages of Constructor Injection:**
- There is normally one parameterized constructor, but if you need more than one dependency for class, you can define multiple constructors to achieve more than one dependency.
- It supports unit testing, because dependencies can be passed through the constructor.
- We can make dependency immutable (to prevent circular dependency) immutable by making the dependency reference final.
- It makes a strong dependency contract.
- It emphasizes the importance of receiving a valid dependency and it’ll throw exception if null parameter is passed to constructor.

**Disadvantages of Constructor Injection:**
- For legacy classes we need to modify the constructor to implement DI, in this case we need to modify all the constructor calls.
- To avoid changing all over the places we can create new constructor and use it for future dependencies, but it’ll create confusion in which situation one should use one of two constructors.
- If the dependency is used rarely and expensive to create, then we should avoid it passing to constructor and better choose Setter injector.

**Example of Constructor Injection:**

Create the `IDbRepository` interface and then create the `DbRepository` by implementing the `IDbRepository` interface.

{% gist https://gist.github.com/sandeepkumar17/e94de56194ce3eea6d3cb982cc715154 %}

{% gist https://gist.github.com/sandeepkumar17/4873ed9fe86caef2d0829d6988282535 %}

## Setter Injection
It is also known as Property Injection. In this we do not need to pass the dependency through the constructor but dependencies are passed through public properties that are exposed.  

**Advantages of Setter Injection:**
- It allows us to create dependency for expensive resources and with this we can create dependency as late as possible and only when we needed it.
- Setter Injection allows us to create dependencies for legacy classes without modifying constructor and related constructor calls.

**Disadvantages of Setter Injection:**
- It is very difficult to identify which dependencies are required and when.
- It makes it a bit more difficult to track down the exception and reason of exception.

**Example of Setter Injection:**

{% gist https://gist.github.com/sandeepkumar17/1d2ac79535edcfd0b42af7b59990baab %}

## Interface Injection
Another way of DI is Interface base Injection. Interface injection allows us to pass the dependency into dependent object in the form of interface, implemented by dependent class. In this interface one method is declared that is implemented by the dependent class. The dependencies are passed using the method's parameters at a time before the dependency is required.

> We can implement Interface Injection by either way, either constructor injection or setter injection. Advantages or disadvantages depend on which DI is selected for interface injection.

**Interface Injection Components:**
- IDependent interface: It defines the methods that inject one or more dependency into dependent class.
- Dependent Class: This class implements the IDependent interface.
- IDependency interface: It includes all dependency members that can be called from the Dependent class. By mean of IDependency interface, we can easily inject any of the class that implements IDependency interface.
- Dependency Class: This class implements IDependency interface and use to substitute into dependent class using Interface injection.

**Example of Interface Injection:**

{% gist https://gist.github.com/sandeepkumar17/ba9e01c909efc5856c1c88d52a808f72 %}

## Advantages of Dependency Injection pattern:
- With the help of DI tight coupling is eliminated.
- DI allows code reusability.
- It improves code readability and maintainability.
- It improves application testing.

## DI Frameworks:
There are a number of DI frameworks available to help the developer. Few of them are mentioned below for usage and further studies:

- **[Spring Framework](http://www.springsource.org/):** A large framework which offers a number of other capabilities apart from DI.
- **PicoContainer:** PicoContainer’s most important feature is its ability to instantiate arbitrary objects. This is done through its API, which is similar to a hash table. You can put java.lang.Class objects in and get object instances back.
- A small tightly focused DI container framework, [read here](http://picocontainer.com/) for more information.
- **Spring.NET:** Spring.NET is an open source application framework that makes building enterprise .NET applications easier, [read here](http://www.springframework.net/) for more details.
- 
- **HiveMind:** Another DI container framework available for developers, [read here](http://hivemind.apache.org/) for more details.
- **StructureMap:** StructureMap is the oldest, continuously used IoC/DI container for .Net dating back to its first public release and production usage all the way back in June 2004 on .Net 1.1. The current 4.* release represents 12+ years of lessons learned in the StructureMap and greater .Net community.
- **Castle Windsor:** Castle Windsor is a best of breed, mature Inversion of Control container available for .NET. Helps you get more done with less code and in less time (but not in the traditional code generator mess that some preach).
- **Autofac:** Autofac is an addictive Inversion of Control container for .NET Core, ASP.NET Core, .NET 4.5.1+, Universal Windows apps, and more. Let Autofac inject your constructor parameters for you. It can also handle property and method injection.
- **Unity:** The Unity Container (Unity) is a lightweight, extensible dependency injection container. It facilitates building loosely coupled applications and provides developers with the following advantages:
- **Ninject:** Stop writing monolithic applications that make you feel like you have to move mountains to make the simplest of changes. Ninject helps you use the technique of dependency injection to break your applications into loosely-coupled, highly-cohesive components, and then glue them back together in a flexible manner.
- **DryIoc:** Designed for low-ceremony use, performance, and extensibility.
- **LinFu:** A framework that adds mixins, inversion of control, DbC, and other language features to the Common Language Runtime.
- **PicoContainer.NET:** PicoContainer’s most important feature is its ability to instantiate arbitrary objects. This is done through its API, which is similar to a hash table. You can put java.lang.Class objects in and get object instances back.
- **S2Container.NET:** S2Container.NET is a lightweight DI container supporting AOP. It is a port of Java version Seasar2 to .NET framework.

That’s it for now guys, I hope you have enjoyed learning DI, your feedback and comments will be highly appreciated !!!

---
_Originally published at [http://diary-techies.blogspot.com](https://diary-techies.blogspot.com/2013/09/dependency-injection-pattern-and.html) on September 09, 2013._
