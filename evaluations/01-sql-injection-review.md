# Evaluation 01: SQL Injection Vulnerability

## Task

Review the following backend code and determine whether it is safe for production.

    const result = await db.query(
      `SELECT * FROM users WHERE email = '${email}'`
    );

    if (!result) {
      return res.status(404).json({ message: "User not found" });
    }

    return res.json(result);

## Requirement Analysis

The code is expected to:

1. Retrieve a user using an email address.
2. Handle the case where no user exists.
3. Avoid security vulnerabilities.
4. Be suitable for production use.

## Problems Identified

### 1. Critical SQL Injection Vulnerability

The application directly interpolates user-controlled input into the SQL statement:

    `SELECT * FROM users WHERE email = '${email}'`

An attacker may manipulate the query by supplying specially crafted input.

This creates a serious security risk because an attacker may alter the intended query and potentially access or manipulate data.

**Severity: Critical**

### 2. Incorrect Missing-User Check

The condition:

    if (!result)

does not necessarily determine whether the query returned any rows.

Database libraries commonly return an object or array even when no matching record exists. The implementation should check the returned row collection according to the database client's API.

**Severity: Major**

## Recommended Fix

Use a parameterized query rather than directly inserting user input into SQL.

Example:

    const result = await db.query(
      "SELECT * FROM users WHERE email = $1",
      [email]
    );

    if (result.rows.length === 0) {
      return res.status(404).json({
        message: "User not found"
      });
    }

    return res.json(result.rows[0]);

> Note: Placeholder syntax varies depending on the database library.

## Production Verdict

**Rejected.**

The direct interpolation of user input creates a critical SQL injection vulnerability. The missing-user logic is also unreliable.

The code should not be approved for production until parameterized queries and correct result handling are implemented.

## Evaluation Summary

| Category | Verdict |
|---|---|
| Requirement Compliance | Partial |
| Correctness | Poor |
| Security | Critical Failure |
| Maintainability | Acceptable |
| Production Approval | Rejected |

## Key Lesson

Code that appears functionally correct can still contain critical security failures. Input must never be treated as safe simply because it is inserted into a query string.
