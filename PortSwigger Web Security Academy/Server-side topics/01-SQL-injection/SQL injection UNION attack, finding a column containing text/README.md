## **SQL injection UNION attack, finding a column containing text. WriteUp**

### Lab description
![img](https://i.ibb.co/brPwVmB/Screenshot-from-2023-01-21-13-41-34.png)
#### **difficulty**: PRACTITIONER
#### **URL**: <https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text>
---

#### **Our task is: "Make the database retrieve the string: 'STRING'"**
*string generates randomly for each lab access.

## Steps
Once if we visit website, select any category, i choice "Pets". 


Now we need to obtain number of columns in this table. Let's using ' ORDER BY [INDEX] ' to sort table by column index. If index bigger than total columns it cause "Internal Server Error". 

<!-- If you already know there is a lot of columns - try simple algorithm to search current number. 

**Example**: total columns is 37. Let's starts with 100 - causes 'Error'. Every time if error do "minus 1" if exist "plus 1" its for proof range. lets divide by 2 = 50 - error, minus 1 - error, 50/2 = 25 - exist, plus 1 - exist, now use 25+(50-25)/2 = 25 + 13 = 38 - error, minus 1 = 37 - exist. Finally we found number of columns. 

Forget that shit, user python automation for this.
 -->

When we enumerated "[INDEX]" we can see items order changes every time. Let's discover changes of indexes **'+ORDER+BY+[INDEX]-- :**
- 1 - first column - **index** in table
- 2 - Name. Sorting by name
- 3 - Price. Sorting by price
- 4 - "Internal Server Error"

URL <https://foo.web-security-academy.net/filter?category=Pets'+ORDER+BY+4--> - causes "Internal Server Error"

> Columns - 3

UNION keyword can be used to retrieve data from other tables within the database. This results in an SQL injection UNION attack.

**Lets submit a of UNION SELECT payload using 3 NULL values:**

https://foo.web-security-academy.net/filter?category=Pets%27+UNION+SELECT+NULL,NULL,NULL--

Now let's finding column containing text. We already known that 1 column is index, it is not text value, 2 - it's item name, lets try it. 

Use this: ' UNION SELECT NULL,'STRING',NULL

https://foo.web-security-academy.net/filter?category=Pets%27+UNION+SELECT+NULL,'STRING',NULL--

![img](https://i.ibb.co/tLJQmLz/Screenshot-from-2023-01-21-14-03-55.png)

> **Congratulations, you solved the lab!**