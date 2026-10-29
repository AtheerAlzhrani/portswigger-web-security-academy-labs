# Listing the Contents of an Oracle Database

- **Platform:** PortSwigger Web Security Academy
- **Difficulty:** Practitioner
- **Category:** SQL Injection
- **Technique:** UNION-based SQL injection
- **Status:** Solved

## Objective
Identify the database table containing usernames and passwords, discover its columns, and retrieve the credentials needed to log in as `administrator`.

## Methodology
1. Determined the number of columns returned by the original query.
2. Identified compatible data types for the UNION query.
3. Enumerated Oracle database metadata to discover the table and column names.
4. Retrieved usernames and passwords from the identified table.
5. Logged in as `administrator` using the retrieved password.

## Impact
An attacker could exploit SQL injection to retrieve sensitive credentials and potentially gain unauthorized access to user accounts.

## Remediation
- Use parameterized queries (prepared statements).
- Avoid constructing SQL queries through string concatenation.
- Apply least-privilege permissions to database accounts.
- Store passwords using a secure password-hashing algorithm.

## Evidence

<img width="1165" height="472" alt="image" src="https://github.com/user-attachments/assets/8e1ae32f-9426-43bf-aeb0-455cfccc7859" />

<img width="1852" height="257" alt="image" src="https://github.com/user-attachments/assets/3419ac23-3f34-444e-b051-e8518967578f" />
