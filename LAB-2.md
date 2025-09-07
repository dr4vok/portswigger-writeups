# 🧠 Lab Title: SQL injection vulnerability allowing login bypass

## 🔍 Category
SQLI

## 🎯 Objective
> Login as administrator via SQLI

## 🧰 Tools Used
- burp suite

## 🚀 Exploitation Steps
#### Observe the description
> We now know that there is a user called administrator

#### Access the lab
> Click on `ACCESS THE LAB`

#### START HACKING!!!
1) Imagine the query  
```sql
SELECT username FROM users WHERE username = '' AND password = ''
```
2) Open burp suite
2.1) Enter wrong credentials \
2.2) intercept the request \
2.3) send to the repeater \
> you should see something like this
`csrf=BcVWAWZC3AOIA7iz0m38MWUWyPS9IvcE&username=df&password=fd` \
We will inject in the username due of the query we have imagined -- Let me explain,
if we did something like
 ```sql
administrator' -- -
```
, this basically means log me in as administrator and comment everything else comes after it (password)

if we try this it will work!

> Let's try it in the browser...and it worked!!!!

### 4. Proof of Success
- We will see that we solved the lab

## 🧵 Reflection
> How to inject in login pages?

