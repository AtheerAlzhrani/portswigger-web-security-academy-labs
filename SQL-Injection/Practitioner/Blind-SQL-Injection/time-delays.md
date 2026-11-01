# Time-Based Blind SQL Injection

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Practitioner
* **Category:** Blind SQL Injection
* **Status:** Solved
* **Database:** PostgreSQL
* **Tool:** Burp Suite

## Objective

Exploit a blind SQL injection vulnerability by triggering a time delay to demonstrate that SQL queries can be executed even when their results are not displayed.

## Methodology

1. Intercepted the application's HTTP request using Burp Suite.
2. Identified the `TrackingId` cookie as a potential SQL injection point.
3. Modified the cookie value to inject a PostgreSQL time-delay function.
4. Sent the modified request using Burp Repeater.
5. Observed that the server response was delayed by approximately 10 seconds.

## Payload

```sql
' || (SELECT pg_sleep(10))--
```

## Vulnerability

The application was vulnerable to time-based blind SQL injection because user-controlled cookie data was incorporated into a SQL query without proper sanitization or parameterization.

## Impact

An attacker could exploit this vulnerability to infer information from the database through response-time differences, even when query results are not returned.

## Remediation

* Use parameterized queries and prepared statements.
* Never concatenate user-controlled input directly into SQL queries.
* Apply proper input validation.
* Follow the principle of least privilege for database accounts.

## Evidence
<img width="895" height="815" alt="image" src="https://github.com/user-attachments/assets/deaf5a14-6c40-41f2-8324-7f5d2aad5d2b" />
<img width="1492" height="251" alt="image" src="https://github.com/user-attachments/assets/5da1cd52-a5f1-45ab-9942-7c432bc15160" />


## Result

Successfully exploited the time-based blind SQL injection vulnerability by triggering a 10-second delay using PostgreSQL's `pg_sleep()` function.
