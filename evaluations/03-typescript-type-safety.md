# Evaluation 03: TypeScript Type Safety

## Task

Review the following TypeScript function and determine whether it provides adequate type safety.

    function getFullName(user: any) {
      return `${user.firstName} ${user.lastName}`;
    }

The function should safely accept a user and return their full name.

## Requirement Analysis

The implementation should:

1. Accept a valid user object.
2. Ensure the required properties exist.
3. Catch invalid usage during development where possible.
4. Provide clear and maintainable type definitions.

## Problem Identified

### Use of `any` Removes Type Safety

The parameter is declared as:

    user: any

Using `any` effectively disables TypeScript's type checking for that value.

The compiler will allow calls that may be invalid:

    getFullName(undefined);

    getFullName({});

    getFullName(123);

These values may cause runtime errors or produce incorrect output.

For example, attempting to access properties on `undefined` can result in an error.

**Severity: Major**

## Recommended Fix

Define the expected structure of the user.

    interface User {
      firstName: string;
      lastName: string;
    }

    function getFullName(user: User): string {
      return `${user.firstName} ${user.lastName}`;
    }

Now TypeScript can validate that the required properties exist before the code is executed.

For example:

    getFullName({
      firstName: "John",
      lastName: "Doe"
    });

This is valid.

However:

    getFullName({});

would produce a TypeScript error because the required properties are missing.

## Why This Is Better

The typed implementation provides:

1. Compile-time error detection.
2. Clear documentation of the expected object structure.
3. Better editor autocomplete and tooling support.
4. Safer refactoring.
5. Reduced risk of invalid values reaching runtime.

## Important Consideration

There are situations where the shape of incoming data is unknown, especially when processing external API responses.

In those cases, `unknown` can be safer than `any` because it requires validation before the value can be used.

For example:

    function processUser(user: unknown) {
      // Validate the value before accessing its properties.
    }

`unknown` does not automatically guarantee safety, but it prevents unrestricted property access without validation.

## Production Verdict

**Rejected in its original form.**

The function may work when valid data is passed, but the use of `any` removes one of the primary benefits of TypeScript: compile-time type checking.

The typed version is more reliable and maintainable.

## Evaluation Summary

| Category | Verdict |
|---|---|
| Requirement Compliance | Partial |
| Type Safety | Major Failure |
| Correctness | Conditional |
| Maintainability | Acceptable |
| Production Approval | Rejected until typed correctly |

## Key Lesson

TypeScript cannot protect code when developers bypass its type system with `any`.

Defining explicit types allows invalid data to be detected earlier and makes code easier to understand, maintain and refactor.
