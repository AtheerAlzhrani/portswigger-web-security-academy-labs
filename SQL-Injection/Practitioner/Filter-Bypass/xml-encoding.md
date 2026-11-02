# SQL Injection – Filter Bypass via XML Encoding

**Platform:** PortSwigger Web Security Academy
**Difficulty:** Practitioner
**Category:** SQL Injection – Filter Bypass
**Status:** Solved
**Tool:** Burp Suite, Hackvertor

## Objective

Bypass a Web Application Firewall (WAF) that blocks SQL injection payloads by using XML encoding to obfuscate the malicious input.

Retrieve the administrator's credentials from the `users` table and log in to the administrator account.

## Methodology

1. Intercepted the stock check request using Burp Suite.
2. Identified the stock check parameter as vulnerable to SQL injection.
3. Tested a UNION-based SQL injection to retrieve information from other database tables.
4. Identified that the WAF blocked requests containing recognizable SQL injection patterns.
5. Used the Hackvertor extension to XML-encode the SQL injection payload.
6. Submitted the encoded payload to bypass the WAF filtering.
7. Successfully retrieved usernames and passwords from the `users` table.
8. Used the administrator credentials to log in and solve the lab.

## Payload

```sql
1 UNION SELECT username || '-' || password FROM users
```

**Encoding technique:** XML encoding using the Hackvertor extension in Burp Suite.

## Vulnerability

The application was vulnerable to UNION-based SQL injection. Although a WAF attempted to block malicious SQL keywords, XML encoding allowed the payload to bypass the filtering mechanism.

## Impact

An attacker could bypass WAF filtering and retrieve sensitive information from database tables, including usernames and passwords.

## Remediation

* Use parameterized queries and prepared statements.
* Never rely on WAF filtering as the primary SQL injection defense.
* Normalize and decode input consistently before security inspection.
* Apply strict server-side input validation.
* Follow the principle of least privilege for database accounts.

## Evidence

*Screenshot showing the XML-encoded payload in Burp Suite and successful retrieval of administrator credentials.*

## Result

Successfully bypassed the WAF using XML encoding, exploited the UNION-based SQL injection vulnerability, retrieved administrator credentials, and authenticated as the administrator.

