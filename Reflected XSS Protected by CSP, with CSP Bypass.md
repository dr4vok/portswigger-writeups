# 🧠 Lab Title: Reflected XSS Protected by CSP, with CSP Bypass

## 🔍 Category

Injections: XSS

## 🎯 Objective

> Call the `alert` function

## 🧰 Tools Used

* Web developer tools

## 🚀 Exploitation Steps

#### Observe the description

This lab is protected by CSP.

**What is CSP?**
Content Security Policy (CSP) is a browser security standard that helps prevent attacks like Cross-Site Scripting (XSS), clickjacking, and data injection.

It works by letting the site owner tell the browser what content is allowed to load and execute. You set this using the `Content-Security-Policy` HTTP response header.

Example:

```http
Content-Security-Policy: default-src 'self'; script-src 'self' https://apis.google.com
```

**What is XSS?**
Cross-Site Scripting (XSS) allows an attacker to execute code on the target website. In modern web applications, JavaScript is the most common vector for these types of injections.

#### Access the lab

> Click on `ACCESS THE LAB`

#### START HACKING!!!

1. Let's test for HTML injection:

```html
<b>test</b>
```

It worked.

Start with simple HTML so you can build up later — that's why we test for HTML injection first.

2. Try a simple script:

```html
<script>alert()</script>
```

You may see in DevTools that the lab doesn't filter the injection, but the alert does not execute for some reason.

We should know that XSS occurs when the website misinterprets user input as source code.

3. If we inspect the HTTP history, we may find another parameter called `token=`.

4. Try this payload:

```html
<script>alert()</script>&token=fdsfsd
```

Search for `fdsfsd` in DevTools. You might not find it in the DOM, but that doesn't mean it's not returned in the response.

Now intercept the responses by going to **Options** → scroll down → tick **Intercept server responses**.

If you look at the intercepted responses, you should see a match for `fdsfsd`.

The reason our injection didn't execute is Content Security Policy. Let's try to bypass it.

We can attempt to inject a CSP directive such as:

```
script-src-elem
```

We will use this to override the original script source and set it to `'unsafe-inline'`.

So the final payload used here is:

```html
<script>alert(33)</script>&token=script-src-elem 'unsafe-inline'
```

## 🧵 Reflection

> How to bypass CSP?
