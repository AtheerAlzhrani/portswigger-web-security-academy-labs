# Determine the Number of Columns

- **Platform:** PortSwigger Web Security Academy
- **Difficulty:** Practitioner
- **Category:** SQL Injection
- **Technique:** UNION-based SQL injection
- **Status:** Solved

## Objective
Determine the number of columns returned by the original SQL query.

## Methodology
1. Intercepted the product category filter request using Burp Suite.
2. Modified the category parameter with a UNION SELECT statement.
3. Tested the following payload:

   `Gifts' UNION SELECT NULL,NULL,NULL--`

4. Used three NULL values to test a UNION query containing three columns.
5. Examined the response to determine whether the query was accepted.

## Result
The payload tested a UNION query with three columns. If the query was accepted without an SQL error, this indicated that the original query returned three columns.

## Impact
An attacker may use the discovered column count to construct UNION queries that retrieve data from other database tables.

## Remediation
- Use parameterized queries (prepared statements).
- Avoid concatenating untrusted input into SQL statements.
- Handle database errors without exposing sensitive implementation details.

## Evidence
<img width="1487" height="716" alt="image" src="https://github.com/user-attachments/assets/858672a4-bbe2-480e-8232-ae906bc3d7c8" />
<img width="1735" height="262" alt="image" src="https://github.com/user-attachments/assets/406911e5-faac-482a-8f85-bc5dfaa96629" />



