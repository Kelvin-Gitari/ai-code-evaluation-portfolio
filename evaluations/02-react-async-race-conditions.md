# Evaluation 02: React Async Race Conditions

## Task

Review the following React code. The component fetches user data whenever `userId` changes.

    useEffect(() => {
      fetch(`/api/users/${userId}`)
        .then((response) => response.json())
        .then((data) => {
          setUser(data);
        });
    }, [userId]);

The component must remain correct when users are switched rapidly.

## Requirement Analysis

The implementation should:

1. Fetch the correct user whenever `userId` changes.
2. Prevent stale data from overwriting newer data.
3. Handle asynchronous operations safely.
4. Remain suitable for production use.

## Problem Identified

### Race Condition and Stale Responses

The dependency array correctly triggers a new request whenever `userId` changes.

However, the implementation does not protect against requests completing out of order.

Consider this sequence:

1. `userId` changes to `10`.
2. Request A starts.
3. The user quickly changes to ID `20`.
4. Request B starts.
5. Request B finishes first and updates the UI with User 20.
6. Request A finishes later and overwrites the state with User 10.

The UI would now display stale data.

**Severity: Major**

## Recommended Fix

One approach is to invalidate the previous request when the effect is cleaned up.

    useEffect(() => {
      let active = true;

      fetch(`/api/users/${userId}`)
        .then((response) => {
          if (!response.ok) {
            throw new Error("Failed to fetch user");
          }

          return response.json();
        })
        .then((data) => {
          if (active) {
            setUser(data);
          }
        })
        .catch((error) => {
          if (active) {
            console.error(error);
          }
        });

      return () => {
        active = false;
      };
    }, [userId]);

## Why This Works

When `userId` changes, React runs the cleanup function for the previous effect before running the next effect.

The previous request may still finish, but it can no longer update the component state because the `active` flag becomes false.

This prevents stale responses from overwriting newer user data.

## Alternative Approach

Another approach is to use `AbortController` to cancel an in-flight request.

    useEffect(() => {
      const controller = new AbortController();

      async function loadUser() {
        try {
          const response = await fetch(`/api/users/${userId}`, {
            signal: controller.signal
          });

          if (!response.ok) {
            throw new Error("Failed to fetch user");
          }

          const data = await response.json();
          setUser(data);
        } catch (error) {
          if (error instanceof Error && error.name !== "AbortError") {
            console.error(error);
          }
        }
      }

      loadUser();

      return () => {
        controller.abort();
      };
    }, [userId]);

## Production Verdict

**Rejected in its original form.**

The original implementation can display stale user data when multiple requests overlap and complete out of order.

The improved implementations protect the component against stale asynchronous updates.

## Evaluation Summary

| Category | Verdict |
|---|---|
| Requirement Compliance | Partial |
| Correctness | Conditional |
| Async Safety | Major Failure |
| Maintainability | Good |
| Production Approval | Rejected until fixed |

## Key Lesson

A correct dependency array does not automatically guarantee asynchronous correctness.

When multiple requests can overlap, the application must explicitly prevent stale or out-of-order responses from updating the UI.
