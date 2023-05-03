---
published: true
title: 'C# 12: New Features and Improvements'
cover_image: 'https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/di-collection-posts/assets/blog-cover/c-sharp.png'
description: 'Discover the new features & improvements in C# 12'
tags: csharp, dotnet, programming, discuss
series:
canonical_url:
---

Some exciting new features come with the **.NET 8 preview** and **C# 12**. General availability of both **C# 12** and **.NET 8** is expected in November.

In this post, we'll take a quick look at some of the major changes in **C# 12**. Developers can access these **C# 12** features in the **Visual Studio 17.6 preview**.

### Primary constructors:
You can now create primary constructors in any `class` and `struct`. With primary constructors, developers can add parameters to the class declaration and use these values inside the class body.

Primary constructors were introduced in **C# 9** as part of the positional syntax for `records`. **C# 12** extends these to all structs and classes.

You can put the parameters after the type name in brackets as shown below:
  ```
  public class Student(int id, string name, IEnumerable<decimal> grades)
  {
      public Student(int id, string name) : this(id, name, Enumerable.Empty<decimal>()) { }
      public int Id => id;
      public string Name { get; set; } = name.Trim();
      public decimal GPA => grades.Any() ? grades.Average() : 4.0m;
  }
  ```
> As I mentioned earlier, the parameters of a primary constructor are in scope throughout the declaring type's entire body. Developers can set up properties or fields or can also utilize them in methods or local functions as variables. These parameters can be provided to a base constructor as well.

### Interpolated Strings Improvements:
Interpolated strings have been around since **C# 6**. In **C# 12**, you can now create dynamic values for strings using complicated expressions.
  ```
  int i = 5;
  string output = $"The value of i is {i}, and its square is {i*i}.";
  Console.WriteLine(output);
  ```
It prints _"The value of i is 5, and its square is 25."_

### Using directives for additional types:
With **C# 12**, developers can use the `using` alias directive to alias any type, not just named types. Semantic aliases can be created for tuple types, array types, pointer types, or other unsafe types.

Here are a few examples:
  ```
  using Measurement = (string Units, int Distance);
  using UnitsInt = int?;
  ```
Aliases usage example:
  ```
  public void Calculation(Measurement measurement)
  { 
    // Method Body
  }
  ```
  
### Lambda Expression Improvements:
**C# 12** empowers lambda expressions by allowing developers to define the default values for parameters. The syntax is identical to that of other default parameters:

For example, `(int incrementTo = 5) => addTo + 1` sets a default value of 5 for the incrementTo parameter, which will be used when there is no value given in the lambda call.
  ```
  var incrementWithDefault = (int incrementTo = 5) => incrementTo + 1;
  incrementWithDefault(); // 6
  incrementWithDefault(9); // 10
  ```

> Besides that, many other enhancements came to lambda expressions to make them more effective.
> - For example, you can now create more complex expressions within lambda functions. 
> - Lambda expressions can now be transformed into expression trees, that simplify complex queries and optimize performance.

### Improved Switch Expressions:
Switch expressions were introduced in **C# 8**, allowing developers to express complex conditional logic concisely and readably. In **C# 12** a new pattern-matching syntax is introduced for switch expressions, which makes writing expressive and concise code even more accessible.

For example, the below switch expression determines whether an integer is positive, negative, or zero.
  ```
  var result = obj switch
  {
      int i when i > 0 => "Positive",
      int i when i < 0 => "Negative",
      _ => "Zero"
  };
  ```

With **C# 12**, we can simplify this code even further as shown below:
  ```
  var result = obj switch
  {
      > 0 => "Positive",
      < 0 => "Negative",
      _ => "Zero"
  };
  ```

### Async Streams:
You can iterate through asynchronous data sources with the new async streams feature of **C# 12**.

This new iterator await foreach help the developers to iterate over a set of async data, see the below example:
  ```
  await foreach (var item in GetItemsAsync())
  {
     Console.WriteLine(item.value);
  }
  ```

### Summary:

These new features of **C# 12** help the developer to create more efficient, expressive, and concise code. **C# 12** helps you to design more potent and reliable applications by reducing the boilerplate code.

Other than these **.NET 8 Preview 3** also includes changes to build paths, workloads, Microsoft.Extensions, and containers. Performance improvements are planned in the **JIT** compiler for **Arm64** and dynamic **PGO** (Profile Guided Optimization) as well.

