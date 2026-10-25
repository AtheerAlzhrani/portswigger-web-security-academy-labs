# SQL Injection — Login Bypass

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Apprentice
* **Status:** Solved

## Objective

Bypass login authentication using SQL injection.

## Testing

* **Tool:** Burp Suite
* **Parameter:** `username`
* **Payload:** `administrator'--`

## Explanation

The apostrophe closes the SQL string, while `--` comments out the remaining query on supported databases. This may bypass the password check.

## Impact

An attacker may gain unauthorized access to another user's account.

## Remediation

Use parameterized queries (prepared statements) to prevent SQL injection.

## Lab

https://portswigger.net/web-security/sql-injection/lab-login-bypass
