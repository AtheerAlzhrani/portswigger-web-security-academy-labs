
# SQL Injection — Querying Database Version on Oracle

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Practitioner
* **Status:** Solved
* **Tool:** Burp Suite
* **Vulnerable parameter:** `category`

## Objective

Retrieve the Oracle database version by exploiting a SQL injection vulnerability in the product category filter.

## Methodology

1. Captured the product category request using Burp Suite.
2. Sent the request to Repeater for manual testing.
3. Used a `UNION SELECT` query to test the number of columns.
4. Queried Oracle's `v$version` view to retrieve database version information.

## Payloads

**Column testing:**

```sql
'+UNION+SELECT+'abc','def'+FROM+dual--
```

**Database version retrieval:**

```sql
'+UNION+SELECT+BANNER,+NULL+FROM+v$version--
```

## Impact

An attacker could exploit SQL injection to retrieve database metadata, identify the database software, and use this information to guide further attacks.

## Remediation

* Use parameterized queries (prepared statements).
* Avoid building SQL queries through string concatenation with untrusted input.
* Apply least-privilege permissions to database accounts.

## Evidence

### Lab Completion

<img width="1710" height="287" alt="image" src="https://github.com/user-attachments/assets/25ca7e8b-6678-489e-8a63-d3f155c3b26c" />


### Retrieved Database Version
<img width="732" height="462" alt="image" src="https://github.com/user-attachments/assets/4f89667b-161f-40df-bf78-5dd6d36a1dc4" />



## Lab Reference

https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle
