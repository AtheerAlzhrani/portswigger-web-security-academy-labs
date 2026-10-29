# Retrieve Data from Other Tables

- **Platform:** PortSwigger Web Security Academy
- **Difficulty:** Practitioner
- **Category:** SQL Injection
- **Technique:** UNION-based SQL injection
- **Status:** Solved
- **Tool:** Burp Suite

## Objective
Exploit a SQL injection vulnerability to retrieve data from another database table.

## Methodology
1. Intercepted the product category filter request using Burp Suite.
2. Determined the number of columns returned by the original query.
3. Used a UNION SELECT statement to retrieve data from another database table.
4. Examined the application's response to verify the retrieved data.

## Payload
<!-- Add the exact payload used in the lab here. -->

## Result
Successfully retrieved data from another database table by exploiting a UNION-based SQL injection vulnerability.

## Impact
An attacker could retrieve sensitive information from database tables that the application does not intend to expose.

## Remediation
- Use parameterized queries (prepared statements).
- Avoid building SQL queries through string concatenation.
- Apply least-privilege permissions to database accounts.

## Evidence
<!-- Add a screenshot showing successful lab completion here. -->
