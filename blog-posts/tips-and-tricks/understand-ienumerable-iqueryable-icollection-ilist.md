---
published: false
title: 'C#: Understand about IEnumerable vs. IQueryable vs. ICollection vs. IList'
cover_image: 'https://github.com/sandeepkumar17/td-dev.to/blob/di-collection-posts/assets/blog-cover/c-sharp.png'
description: 'Understand about IEnumerable vs. IQueryable vs. ICollection vs. IList and review the differences'
tags: csharp, linq, collection, programming 
series:
canonical_url:
---

In this article, we’ll understand the interfaces (`IEnumerable`, `IQueryable`, `ICollection`, and `IList`) available for holding and querying the data.

### IEnumerable: 
- IEnumerable exists in `System.Collections` Namespace.
- IEnumerable is the most generic item of all and a core interface that is used to iterate over a collection of the specified type.
- IEnumerable provides an Enumerator for accessing the collection.
- IEnumerable is forward only collection like LinkedList. It doesn’t move between items or backward, i.e. one can't get at the fifth item without passing the first four items.
- It is a read-only collection and it doesn't support adding or removing items.
- IEnumerable is best to query data from in-memory collections like List, Array, etc.
- It mainly implements two methods:
    - MoveNext`: This method tells whether there are more records to move on or not.
    - `GetCurrent`: This method returns the current record from the collection.
- IEnumerable still might use deferred execution and also supports further filtering.
- IEnumerable does not run query until it is requested by iteration or enumerator.
- Using IEnumerable we can find out the no of elements in the collection after iterating the collection.

### IQueryable: 
- IQueryable exists in `System.Linq` namespace.
- IQueryable extends the `IEnumerable` interface.
- IQueryable allows deferred query execution i.e. query generated through IQueryable isn't executed until you iterate, enumerate, or add any method such as `ToList()`, `First()`, `Single()`, etc.
- IQueryable enables a variety of interesting deferred execution scenarios such as paging and composition-based queries.
- IQueryable best suits remote data sources, like a DB or web service.
- IQueryable is particularly used for LINQ queries.
- IQueryable doesn’t support custom comparer, like any sting method such as `string.IsNullOrWhiteSpace` etc.
- IQueryable is not a good place to handle errors, so one must take care while writing complex queries.

### ICollection: 
- ICollection exists in `System.Collections` Namespace.
- ICollection implements the `IEnumerable` interface.
- ICollection is the base of `Collection<T>`, `IList<T>`, and `IDictionary` objects.
- It is considered the most basic type for collections and is used to manipulate generic collections.
- As it implements the `IEnumerable` interface so it also implements methods `MoveNext` and `GetCurrent`, so with this interface you can iterate through the collection.
- ICollection also has its own methods (apart from `IEnumerable` methods) like:
    - Add: It adds a record at the end of the collection
    - Remove: It removes the specified item from the collection
    - Contains: It’s a boolean type method that tells whether the collection contains the specified item or not.
- Some collections that limit access to their elements, i.e. Queue or Stack class, directly implement the ICollection interface.
- ICollection is normally used where we define EF table relationships and we use this in the virtual keyword.
- ICollection doesn’t support indexing as IList does.

### IList:
- IList exists in `System.Collections` Namespace.
- IList implementations fall into three categories:
    - `Read-only`: A read-only `IList` cannot be modified.
    - `Fixed-size`: A fixed-size `IList` does not allow the addition or removal of elements, but it allows the modification of existing elements.
    - `Variable-size`: A variable-size `IList` allows the addition, removal, and modification of elements.
- IList is used where you need to iterate (read), modify and sort, order a collection
- With IList random element access is allowed i.e. you can access an element in a specific index in a list. For e.g. you can directly access an element at index 10 instead of first iterating through 0-9 elements.
- Like IEnumerable, IList is also in memory collection and helps you to query data from in-memory collections like List, Array etc.
- IList implements two interfaces `ICollection` and `IEnumerable`. So it also implements the methods of both the interfaces.
- IList also has its own methods like:
    - `Insert`: It inserts the given item at the specified Index.
    - `RemoveAt`: It removes the item from the specified Index.
    - `IndexOf`: It retrieves the item from the specified Index.
- IList can give you the no of elements in the collection without iterating the collection.
- IList supports deferred execution, but it doesn't support further filtering.
- IList supports custom comparer as the comparison is done inside the memory.

<div style="text-align:center">
  <img src="https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/assets/summary.png" />
</div>

In this post, I tried to explain some of the basic differences between `IEnumerable`, `IQueryable`, `ICollection`, and `IList` interfaces.

I hope this article will help you to choose a specific interface based on your demand. You can share your feedback, question, or comments about this article.
  
---
_Originally published at [http://diary-techies.blogspot.com](https://diary-techies.blogspot.com/2014/03/c-understand-about-ienumerable-vs.html) on 
March 14, 2014._
