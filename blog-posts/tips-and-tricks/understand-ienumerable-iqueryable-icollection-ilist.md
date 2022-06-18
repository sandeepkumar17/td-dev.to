---
published: true
title: 'C#: Understand about IEnumerable vs. IQueryable vs. ICollection vs. IList'
cover_image: 'https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/assets/blog-cover/tips_tricks.png'
description: 'Understand about IEnumerable vs. IQueryable vs. ICollection vs. IList and review the differences'
tags: csharp, linq, collection, programming 
series:
canonical_url:
---

In this article we’ll understand about the interfaces (IEnumerable, IQueryable, ICollection and IList) available for holding and querying the data.


#### IEnumerable: 
- IEnumerable exists in System.Collections Namespace.
- IEnumerable is most generic item of all and a core interface which is used to iterate over collection of specified type.
- IEnumerable provides Enumerator for accessing collection.
- IEnumerable is forward only collection likes LinkedList. It doesn’t move between items or backward, i.e. one can't get at fifth item without passing first four items.
- It is read-only collection and it doesn't support add or remove items.
- IEnumerable is best to query data from in-memory collections like List, Array etc.
- It mainly implements two methods:
    - MoveNext: This method tells whether there are more records to move on or not.
    - GetCurrent: This method returns the current record from the collection.
- IEnumerable still might use deferred execution and also supports further filtering.
- IEnumerable does not run query until it is requested by iteration or enumerator.
- Using IEnumerable we can find out the no of elements in the collection after iterating the collection.

#### IQueryable: 
- IQueryable exists in System.Linq namespace.
- IQueryable extends IEnumerable interface.
- IQueryable allows deferred query execution i.e. query generated through IQueryable isn't executed until you iterate, enumerate or add .ToList() method.
- IQueryable enables a variety of interesting deferred execution scenarios such as paging and composition based queries.
- IQueryable best suits for remote data source, like a DB or web service.
- IQueryable is particularly used for LINQ queries.
- IQueryable doesn’t support custom comparer, like any sting method such as ‘string.IsNullOrWhiteSpace’ etc.
- IQueryable is not a good place to handle errors, so one must take care while writing complex queries.

ICollection: 
- ICollection exists in System.Collections Namespace.
- ICollection implements IEnumerable interface.
- ICollection is base of Collection<T>, IList<T> and IDictionary objects.
- It is considered the most basic type for collections and used to manipulate generic collections.
- As it implements IEnumerable interface so it’s also implements methods MoveNext and GetCurrent, so with this interface you can iterate through collection.
- ICollection also having its own methods (apart from IEnumerable methods) like:
    - Add: It adds record at the end of collection
    - Remove: It removes specified item from collection
    - Contains: It’s a boolean type method which tells whether collection contains the specified item or not.
- Some collections that limit access to their elements, i.e. Queue or Stack class, directly implement the ICollection interface.
- ICollection is normally used where we define EF table relationships and we use this in virtual keyword.
- ICollection doesn’t support indexing like IList does.

IList: 
- IList exists in System.Collections Namespace.
- IList implementations fall into three categories:
    - Read-only: A read-only IList cannot be modified.
    - Fixed-size: A fixed-size IList does not allow the addition or removal of elements, but it allows the modification of existing elements.
    - Variable-size: A variable-size IList allows the addition, removal and modification of elements.
- IList is used where you need to iterate (read), modify and sort, order a collection
- With IList random element access is allowed i.e. you can access an element in a specific index in a list. For e.g. you can directly access an element at index 10 instead of first iterating through 0-9 elements.
·- Like IEnumerable, IList is also in memory collection and helps you to query data from in-memory collections like List, Array etc.
- IList implements two interfaces ICollection and IEnumerable. So it’s also implements the methods of the both the interfaces.
- IList also having its own methods like:
    - Insert: It insert the given item at specified Index.
    - RemoveAt: It removes the item from specified Index.
    - IndexOf: It retrieves the item from specified Index.
- IList can give you the no of elements in the collection without iterating the collection.
- IList supports deferred execution, but it doesn't support further filtering.
- IList supports custom comparer as comparison is done inside the memory.

<div style="text-align:center">
  <img src="https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/assets/summary.png" />
</div>

In this post I tried to explain some of the basic difference between IEnumerable, IQueryable, ICollection and IList interfaces. I hope this article will help you to choose specific interface based on your demand. You can share your feedback, question or comments about this article.

You might have sometime faced the below exception while attempting to update an entity property:

> System.Data.Linq.ForeignKeyReferenceAlreadyHasValueException: Operation is not valid due to the current state of the object.

---
_Originally published at [http://diary-techies.blogspot.com](https://diary-techies.blogspot.com/2014/03/c-understand-about-ienumerable-vs.html) on 
March 14, 2014._
