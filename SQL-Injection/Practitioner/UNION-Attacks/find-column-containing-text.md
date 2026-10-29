# Find a Column Containing Text

- **Platform:** PortSwigger Web Security Academy
- **Difficulty:** Practitioner
- **Category:** SQL Injection
- **Technique:** UNION-based SQL injection
- **Status:** Solved
- **Tool:** Burp Suite

## Objective
Identify which column in the original SQL query can display the random text provided by the lab.

## Methodology
1. Intercepted the product category filter request using Burp Suite.
2. Confirmed that the original query returned three columns.
3. Replaced the NULL values one at a time with the required text.
4. Used the following payload:

   `Gifts' UNION SELECT NULL,'pzO5Wc',NULL--`

5. Observed the response to identify the column that displayed the injected text.

## Result
The injected value `pzO5Wc` appeared in the application's response, identifying the second column as compatible with text and solving the lab.

## Impact
An attacker could exploit a UNION-based SQL injection vulnerability to retrieve and display data from other database tables.

## Remediation
- Use parameterized queries (prepared statements).
- Avoid constructing SQL queries through string concatenation.
- Handle database errors safely without exposing sensitive information.

## Evidence
<!-- Add a screenshot showing the successful lab completion here. -->
