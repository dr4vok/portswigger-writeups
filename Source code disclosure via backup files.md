# 🧠 Lab Title: Source code disclosure via backup files
## 🔍 Category
information disclosure

## 🎯 Objective
> identify and submit the database password

## 🧰 Tools Used
- your mind

## 🚀 Exploitation Steps
#### Observe the description
We got to know that this lab contains information disclosure , which falls into cryptographic failure in the OWASP top 10, this vulnerabilities allow to access inforamtions that shouldnt be attainable or visible to you

#### Access the lab
> Click on `ACCESS THE LAB`

#### START HACKING!!!
1) search for /robots.txt , but before **what is robots.txt?** its a file that tells search engines to not display specific file path 

2) we found out that there is a /backup file  , lets check it out 

3) we found a the database that we been told about , lets access it and...we got the password , you will find it under 
```sql
ConnectionBuilder connectionBuilder = ConnectionBuilder.from
```
## 🧵 Reflection
> what is information disclousre vulnerability? \ 
> What is /robots.txt ? 
