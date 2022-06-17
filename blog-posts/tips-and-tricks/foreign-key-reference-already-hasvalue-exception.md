---
published: true
title: 'System.Data.Linq.ForeignKeyReferenceAlreadyHasValueException: Operation is not valid due to the current state of the object.'
cover_image: 'https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/assets/blog-cover/tips_tricks.png'
description: 'System.Data.Linq.ForeignKeyReferenceAlreadyHasValueException: Operation is not valid due to the current state of the object.'
tags: csharp, dotnet, linq, programming 
series:
canonical_url:
---

You might have sometime faced the below exception while attempting to update an entity property:

> System.Data.Linq.ForeignKeyReferenceAlreadyHasValueException: Operation is not valid due to the current state of the object.

#### Reason:
After looking into the issue I found that this error occurs when an attempt is made to change a foreign key when the entity is already loaded.
In this case, I was trying to assign a value to the foreign key property, and an exception is thrown as foreign key fields and association properties don’t match, when changes are submitted.

In such a scenario there are two values, one in the foreign key field or the one on the other side of the relationship and it doesn’t know which value is correct, and as a result exception is thrown.

#### Solution:
To avoid the exception the best way to update the relationship is by changing the association property and not the foreign keys. And then it will automatically keep the foreign keys in sync when you assign the association property a value.

The foreign key field value is changed only when there is either no association property defined on the object or the association property was never assigned a value.

**For Example** we have Address object and Country as it's child object, with foreign column is CountryId:

Then if we do

```
dbContext.Address.CountryId = newCountryId;
```

it'll raise exception, to avoid this update the relationship by changing the association property as:
```
dbContext.Address.Country = db.Country.Single(c => c.Id = newCountryId);
```

---
_Originally published at [http://diary-techies.blogspot.com](https://diary-techies.blogspot.com/2014/03/systemdatalinqforeignkeyreferencealread.html) on 
March 20, 2014._
