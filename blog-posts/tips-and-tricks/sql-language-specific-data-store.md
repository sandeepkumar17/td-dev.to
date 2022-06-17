---
published: true
title: 'SQL Server: Storing language specific data in SQL Server table field.'
cover_image: 'https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/assets/blog-cover/sql-server.png'
description: 'SQL Server: Storing language specific data in SQL Server table field.'
tags: sql, database, productivity, programming
series:
canonical_url:
---

While working on multi-language support we need to insert different language data in the same field or different fields of a SQL Server table.

There are two basic rules one needs to keep in mind while storing multi-lingual or Unicode data:
- First, the column must be of Unicode data type (i.e., `nchar`, `nvarchar`, `ntext`).
- And second is that the value must be prefixed with N while insertion.

Below is the sample script with output to understand it better.

{% gist https://gist.github.com/sandeepkumar17/938d940ba0030013d42ffeb9a6b1041b %}

---
_Originally published at [http://diary-techies.blogspot.com](https://diary-techies.blogspot.com/2016/07/sql-server-storing-language-specific.html) on 
July 19, 2016._
