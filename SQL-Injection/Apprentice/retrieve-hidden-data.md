# SQL Injection — Retrieve Hidden Data

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Apprentice
* **Status:** Solved (after confirmation)

## Objective

Retrieve hidden products by exploiting SQL injection in the product category filter.

## Testing

* **Tool:** Burp Suite
* **Parameter:** `category`
* **Payload:** `'+OR+1=1--`

## Explanation

The payload adds an always-true condition and comments out the remaining SQL query, potentially bypassing the category and product-release restrictions.

## Impact

The application may disclose products that should not be visible.

## Remediation

Use parameterized queries and enforce appropriate access controls.

## Lab

[Retrieve hidden data — PortSwigger](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)

