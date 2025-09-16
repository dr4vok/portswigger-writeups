# 🧠 Lab Title: Information Disclosure on Debug Page

## 🔍 Category

Information Disclosure

## 🎯 Objective

> Obtain and submit the `SECRET_KEY` environment variable

## 🧰 Tools Used

* Any tool that can brute-force directories (ffuf in this example)

## 🚀 Exploitation Steps

#### Observe the description

This lab has a page that exposes sensitive data. We should retrieve it using a fuzzer.

> **What is a fuzzer?**
> It’s basically an automated discovery tool. In our case, it’s used to discover directories.

#### Access the lab

> Click on `ACCESS THE LAB`

#### START HACKING!!!

1. Write this command down:

```bash
ffuf -w /usr/share/wordlists/dirb/common.txt -u https://YOUR-LAB-URL/FUZZ -mc 200
```

**What is ffuf?**
Fuzz Faster U Fool (ffuf) is a tool that lets you fuzz for directories. It’s fast, making it a popular choice for security professionals (although some professionals write their own custom tools).

We found a directory called `cgi-bin/`.

2. Enter the directory. It might look messy, but that’s normal. We are only here for the `SECRET_KEY`, so we can use CTRL+F to search for it—and we found it!

## 🧵 Reflection

> How to use ffuf?
> How to locate sensitive data in directories using automated discovery tools.

---

