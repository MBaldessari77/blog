---
title: "Filter records based on theirs age with T-SQL"
layout: default
date: 2017-02-24
categories: t-sql tips
---

## Some example to delete log table row based on theirs age

```sql
DELETE FROM Logs WHERE Logged<DATEADD(mm,-1,GETDATE()) --Delete records older than one month
DELETE FROM Logs WHERE Logged<DATEADD(dd,-1,GETDATE()) --Delete records older than one day
DELETE FROM Logs WHERE Logged<DATEADD(hh,-1,GETDATE()) --Delete records older than one hour
```

> Note that the T-SQL [DATEADD](https://msdn.microsoft.com/en-us/library/ms186819.aspx) function can be applied to [Date and Time Data Types](https://msdn.microsoft.com/en-us/library/ms186724.aspx)
