# 🧠 Lab Title: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

## 🔍 Category
SQL Injection 

## 🎯 Objective
> Make the application display one or more hidden products

## 🧰 Tools Used
- Burp Suite

## 🧵 Vulnerability Overview
> Structure query language injection (SQLI) is a vulnerability that occurs in web application when an attacker is able to insert (Or inject) malicious SQL code into a query that sent to a database.
It happens when user input is not properly validated before being included in a SQL query.

## 🚀 Exploitation Steps
#### observe the lab content
> We know that there is a vulnerability in category filter 
> We got the query "SELECT * FROM products WHERE category = 'Gifts' AND released = 1" which is something i want you to keep always in mind -- imagine the query before injecting , this helps you to organize your payload

#### Launch the lab
> Click on `ACCESS THE LAB` button

#### START HACKING!!!
> We start our vulnerability analysis (in SQLI) with ' , let's test it and observe the output!

> We got an internal server error, which is a good sign—it means the query broke! -- Let's test if we can inject a simple Boolean-Based SQLI 
```sql
'OR 1=1 -- -
```
> Explanation of the payload:
1) First we add our single quote to test for SQL injection , so the query looks something like this:
```sql
SELECT * FROM products WHERE category = 'Gifts' ' AND released = 1
```
as you can notice there is extra ' which causes the internal server error

2) We tested with a simple sql logical `OR` query and `1=1` evaluates true so we are saying the first statement is right or the other one is true which the second one is the true

3) the `-- -` called comment , comment is basically telling the program that executes code to skip this

## 🧵 Reflection
> What is SQLI? \
> Why it occurs? \
> How to approach SQLI? \
> How to inject simple payload?