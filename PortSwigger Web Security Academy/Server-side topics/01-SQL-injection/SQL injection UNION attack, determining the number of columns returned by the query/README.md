## **SQL injection UNION attack, determining the number of columns returned by the query. WriteUp**

### Lab description
![img](https://i.ibb.co/gtdw00m/Screenshot-from-2023-01-11-22-46-36.png)
#### **difficulty**: PRACTITIONER
#### **URL**: <https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns>
---

## Steps
Once if we visit website, select any category, i choice "Lifestyle". 

Now we need to obtain number of columns in this table. Let's using ' ORDER BY [INDEX] ' to sort table by column index. If index bigger than total columns it cause "Internal Server Error". 

<!-- If you already know there is a lot of columns - try simple algorithm to search current number. 

**Example**: total columns is 37. Let's starts with 100 - causes 'Error'. Every time if error do "minus 1" if exist "plus 1" its for proof range. lets divide by 2 = 50 - error, minus 1 - error, 50/2 = 25 - exist, plus 1 - exist, now use 25+(50-25)/2 = 25 + 13 = 38 - error, minus 1 = 37 - exist. Finally we found number of columns. 

Forget that shit, user python automation for this.
 -->

When we enumerated "[INDEX]" we can see items order changes every time. Let's discover changes of indexes **'+ORDER+BY+[INDEX]-- :**
- 1 - first column - **index** in table
- 2 - Name. Sorting by name
- 3 - Price.
- 4 - "External Server Error"

URL <https://foo.web-security-academy.net/filter?category=Lifestyle'+ORDER+BY+4--> - causes "Internal Server Error"

> Columns - 3

UNION keyword can be used to retrieve data from other tables within the database. This results in an SQL injection UNION attack.

**Lets submit a of UNION SELECT payload using 3 NULL values:**

https://foo.web-security-academy.net/filter?category=Lifestyle%27+UNION+SELECT+NULL,NULL,NULL--

When the number of nulls matches the number of columns, the database returns an additional row in the result set, containing null values in each column

![img](https://i.ibb.co/x1y83dP/Screenshot-from-2023-01-11-23-38-35.png)

> **Congratulations, you solved the lab!**