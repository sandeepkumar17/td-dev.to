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

In the output, you can see that values in Var_Field are saved as ????, even though in script N is prefixed while inserting. This is because the field is of `VARCHAR` type.

| Id | Var_Field | NVar_Field |
| --- | --- | --- |
| 1 | ?? ?? ???? ?? | यह एक शब्द है |
| 1 | ?? ??? ???? ?? ????? ?? | ਇਹ ਇੱਕ ਸ਼ਬਦ ਦਾ ਹੁੰਦਾ ਹੈ |

#### Note:
If you are using a stored procedure to insert the Unicode data, then make sure that N is used before the string while passing the parameter to the stored procedure. Also, make sure the parameter declared in the stored procedure is of Unicode type.

---
_Originally published at [http://diary-techies.blogspot.com](https://diary-techies.blogspot.com/2016/07/sql-server-storing-language-specific.html) on 
July 19, 2016._
