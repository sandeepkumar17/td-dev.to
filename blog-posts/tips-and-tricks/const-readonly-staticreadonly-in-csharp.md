---
published: true
title: 'C#: Const vs. readonly vs. static readonly comparison'
cover_image: 'https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/di-collection-posts/assets/blog-cover/c-sharp.png'
description: 'Understand about Const, readonly and static readonly and review the differences'
tags: csharp, dotnet, programming, discuss
series:
canonical_url:
---

In this article, we’ll see the different ways to hold constant or static values in C#.

### const:
In C#, you can declare a const of any type as long as the value assigned can be fully evaluated at compile time. A constant member is defined at compile time and cannot be changed at runtime. Constants are declared as a field using the const keyword, and must be initialized along with its declaration as shown below:
  ```
  public class TestClass
  {
    public const double DaysInWeek = 7;
    public const string FullNameFormat = "Mr." + "{0} {1}";
    public const string FullNameFormatEmpty = string.Empty;  //Invalid because of string.Empty evaluated on runtime
  }
  ```
  
- Constants are Static by default, Constants are accessed as if they were static fields, and we can’t use `static` keywords with `const`.
- The constant cannot be changed in the application anywhere else in the code as this will cause a compiler error.
- Constants must be a value type (`byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `char`, `float`, `double`, `decimal`, or `bool`), an enumeration.
- String and a reference to `null` are the two types of reference values that can be determined at compile time. So, any other reference type can be declared `const` as long as the value you assign to it is null – which turns out to be fairly useless.
- Must have compilation-time value (i.e.: you can have `"A"+"B"` but cannot have method calls)
- The scope of your constant is limited to just one assembly, class, or smaller unit. To use a constant outside of the class that it is declared in, you must fully qualify it using the class name.
- If the `const` is used outside the assembly it is defined in, we must be wary! Because the const’s value is substituted at compile time, this means that if the assembly that defines the `const` changes its value, but the calling assembly isn’t recompiled, then the calling assembly will still see the original value.
- Can be used in attributes
- Are copied into every assembly that uses them (every assembly gets a local copy of values)
- It could be declared within functions
- So `const` should be used mainly for values that are not subject to change, or free if the scope of the const is limited to the same assembly or smaller.
- Use `const` when: The constant is a compile-time value, and the constant's domain does not change or is limited in scope to a single assembly, class, or method.
- We can't use `const` with `string.Empty` value assigned as it’s evaluated on runtime.


### readonly:
A `readonly` member is like a constant in that it represents an unchanging value. The difference is that a `readonly` member can be initialized at runtime, in a constructor as well as able to be initialized as they are declared as shown below.
  ```
  public class TestClass
  {
    public readonly double DaysInWeek = 7;
    public readonly int TotalMonths;

    public TestClass()
    {
      TotalMonths = 12;
    }
  }
  ```
- `readonly` members are not implicitly static, and therefore the static keyword can be applied to a readonly field explicitly if required.
- A `readonly` field can be initialized either at the declaration or in a constructor, `readonly` fields can have different values depending on the constructor used.
- Readonly can be applied to reference type since it's resolved at run-time; you can make struct and class instances `readonly` as well. For example:  
  ```
  public class TestClass
  {
    public static readonly Point Point_Struct = new Point { X = 10, Y = 20 };
  }
  ```
- Since `readonly` fields are not limited to values that can be determined at compile-time, this means that they are not substituted where they are used at compile-time but are the same as other variable lookups. This means that you won't have a situation like with `const` where a class library can change a `const` and another assembly using it won't get the updated value.
- It must have a set value by the time constructor exits
- A `readonly` gives a level of safety higher than const for constants that are subject to change over time. So, things that are truly const (days per week, etc.) can be safely made const because they never, ever change. But things that may be constant now but could conceivably change in the future due to changes in requirement or policy should be made readonly instead, especially if they are visible from other assemblies.
- Because `readonly` fields are looked up at run-time, they are also slightly less efficient than compile-time const. But this performance gain is negligible and you should choose safety over performance.
- If your `readonly` field is a value type (primitive or struct) or is immutable (the type that can't be changed once assigned like string), that field will behave like a constant.
- Use `readonly` when the constant is a struct, a non-string, non-null class, cannot be determined at compile-time, or the constant is instance-level (instead of static).
- `readonly` is evaluated when the instance is created
- A `readonly` member can hold a complex object by using the new keyword at initialization.
- One final thing that makes `readonly` nice is that it can be applied to instance members as well as class (static) members. This means that you can provide a constant that applies to all instances of the class, or just to the current instance.

### static readonly:
`static readonly` is similar to readonly, but it is used to declare a `static` field that can only be initialized once, either at the time of declaration or in a static constructor.
  ```
  public class TestClass
  {
    public static readonly Test testObj = new Test();
  }
  ```
- `static readonly` is typically used if the type of the field is not allowed in a `const` declaration, or when the value is not known at compile time.
- The difference is that the value of a static readonly field is set at run time, and can thus be modified by the containing class, whereas the value of a `const` field is set to a compile-time constant.
- Are evaluated when code execution hits class reference (i.e.: new instance is created or `static` method is executed)
- Must have evaluated value by the time the `static` constructor is done
- You do not want to put ThreadStaticAttribute on these (since the static constructor will be executed in one thread only and it will set value for its thread; all other threads will have this value uninitialized)
- In the `static readonly` case, the containing class is allowed to modify it only in the variable declaration (through a variable initializer) or in the static constructor (instance constructors, if it's not static)

### Example: Showing declaration, usage, and explanation:
Here is an example of using const, readonly, and static readonly in C#:

{% gist https://gist.github.com/sandeepkumar17/0f6905c8bd9f7ecc9cfd98d8d096e322 %}

  ```
  class Example
  {
      // Declare a constant field
      public const int MaxValue = 100;

      // Declare a readonly field
      public readonly int MinValue;

      // Declare a static readonly field
      public static readonly double Pi = 3.14159;

      public Example()
      {
          // Assign a value to the readonly field in the constructor
          MinValue = 0;
      }
  }

  class Program
  {
      static void Main()
      {
          Example ex = new Example();
          // The following line will generate a compile-time error because you cannot change the value of a const field
          // ex.MaxValue = 200;
          // The following line will generate a compile-time error because you cannot change the value of a readonly field
          // ex.MinValue = -100;
          // The following line will generate a compile-time error because you cannot change the value of a static readonly field
          // Example.Pi = 3.14;
      }
  }
  ```

> In this example, `MaxValue` is a constant field with a value of 100, which cannot be modified. `MinValue` is a readonly field, which is initialized to 0 in the constructor and cannot be updated later.
> `Pi` is a static readonly field with a value of 3.14159, which can only be initialized once and cannot be updated later.

### In summary:

- `const`: Value can only be assigned at the time of declaration and cannot be modified later.
- `readonly`: Value can be assigned either at the time of declaration or in a constructor and cannot be modified later.
- `static readonly`: Same as readonly, but for a static field that can only be initialized once, either at the time of declaration or in a static constructor.

**Cheers friend!!!**

---
_Originally published at [http://diary-techies.blogspot.com](https://diary-techies.blogspot.com/2012/09/const-vs-readonly-vs-static-readonly.html)_
