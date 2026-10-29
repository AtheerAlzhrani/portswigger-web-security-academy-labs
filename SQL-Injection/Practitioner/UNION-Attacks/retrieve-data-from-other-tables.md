# Retrieve Data from Other Tables

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Practitioner
* **Category:** SQL Injection
* **Technique:** UNION-based SQL injection
* **Status:** Solved
* **Tool:** Burp Suite

## Objective

Retrieve data from another database table by exploiting a SQL injection vulnerability.

## Methodology

1. Intercepted the product category filter request using Burp Suite.
2. Identified the number of columns required for the UNION query.
3. Injected a UNION SELECT statement to retrieve usernames and passwords from the `users` table.
4. Examined the application's response and identified the administrator's credentials.

## Payload

```sql
' UNION SELECT username,password FROM users--
```

## Result

The injected query returned data from the `users` table. The response included the `administrator` username and its associated password, confirming that the vulnerability could expose sensitive database information.

## Impact

An attacker could retrieve usernames and passwords from database tables, potentially enabling account compromise.

## Remediation

* Use parameterized queries (prepared statements).
* Avoid constructing SQL queries through string concatenation.
* Store passwords using strong, salted password hashes.
* Apply least-privilege permissions to database accounts.

## Evidence
<img width="1267" height="575" alt="image" src="https://github.com/user-attachments/assets/876de134-1aa3-4e7e-ab67-8c6c609e4334" />

<img width="1572" height="252" alt="image" src="https://github.com/user-attachments/assets/c8f57ccd-5e9b-44e1-aa74-af2ffefec603" />




