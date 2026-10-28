# Listing the Contents of a Non-Oracle Database

* **Platform:** PortSwigger Web Security Academy
* **Difficulty:** Practitioner
* **Category:** SQL Injection
* **Technique:** UNION-based SQL injection
* **Status:** Solved

## Objective

Identify the database table containing usernames and passwords, discover its columns, and retrieve the credentials to log in as `administrator`.

## Methodology

1. Determined the number of columns returned by the original query and identified compatible data types.
2. Enumerated the database schema to discover the table containing user credentials.
3. Identified the username and password columns.
4. Used a UNION-based SQL injection to retrieve the stored credentials.
5. Logged in as `administrator` using the retrieved password.

## Impact

An attacker could retrieve sensitive user credentials from the database and use them to gain unauthorized access to user accounts.

## Remediation

* Use parameterized queries (prepared statements).
* Avoid dynamically constructing SQL queries from user input.
* Apply least-privilege permissions to database accounts.
* Protect stored passwords with a suitable password-hashing algorithm.

## Evidence
<img width="1165" height="472" alt="image" src="https://github.com/user-attachments/assets/e5fe235d-d7fc-41cd-bb64-749e4457388d" />

<img width="1640" height="282" alt="image" src="https://github.com/user-attachments/assets/ffee39f4-c4fc-4eac-9148-e42792951b4c" />



