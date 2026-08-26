# Evaluation 05: AI Model Comparison

## Task

Evaluate two AI-generated React implementations for a button that increments a counter.

The goal is to determine whether the implementations correctly satisfy the requirement and which implementation is preferable for production.

## Model A

    function Counter() {
      const [count, setCount] = useState(0);

      return (
        <button onClick={() => setCount(count + 1)}>
          Count: {count}
        </button>
      );
    }

## Model B

    function Counter() {
      const [count, setCount] = useState(0);

      return (
        <button onClick={() => setCount((previous) => previous + 1)}>
          Count: {count}
        </button>
      );
    }

## Evaluation Criteria

The implementations are evaluated using the following criteria:

1. Requirement compliance.
2. Functional correctness.
3. State update safety.
4. Code quality and readability.
5. Production readiness.

## Model A Evaluation

### Requirement Compliance

Model A correctly creates a button that increments and displays a counter.

**Verdict: Pass**

### Correctness

For this simple synchronous click handler, the implementation behaves correctly.

Each click calculates the next value from the current render's `count` value.

**Verdict: Pass**

### Potential Limitation

The update:

    setCount(count + 1)

depends on the value captured by the current render.

For a single, straightforward click update, this is not a practical problem.

However, when a new state depends on the previous state, React's functional update form can be safer and more resilient if multiple updates are queued.

### Code Quality

The implementation is short, readable and directly satisfies the requested behaviour.

**Score: 9/10**

## Model B Evaluation

### Requirement Compliance

Model B correctly creates a button that increments and displays a counter.

**Verdict: Pass**

### Correctness

The implementation uses a functional state update:

    setCount((previous) => previous + 1)

This calculates the next state from the previous state value supplied by React.

**Verdict: Pass**

### State Update Safety

The functional update form is generally preferable when the next state depends on the previous state.

It avoids relying directly on a value captured by the current render and provides a safer pattern if the update logic later becomes more complex.

### Code Quality

The implementation remains concise and readable while using a robust state update pattern.

**Score: 10/10**

## Comparison

| Category | Model A | Model B |
|---|---|---|
| Requirement Compliance | Pass | Pass |
| Functional Correctness | Pass | Pass |
| State Update Pattern | Good | Better |
| Readability | Excellent | Excellent |
| Production Readiness | Yes | Yes |

## Final Verdict

Both models successfully satisfy the original requirement.

Model A should not be considered incorrect. For this specific component, it will behave correctly in normal use.

However, Model B is the stronger implementation because it uses the functional state update pattern when calculating new state from previous state.

The difference is small in this example, and penalising Model A as a major failure would be unnecessarily strict.

## Production Approval

**Model A: Approved**

The implementation is correct for the stated requirement.

**Model B: Approved and Preferred**

Model B provides a slightly more robust state update pattern without adding unnecessary complexity.

## Key Lesson

AI model evaluation should distinguish between:

1. Incorrect code.
2. Correct code with limitations.
3. Correct code that follows stronger engineering practices.

A better implementation does not automatically mean that an alternative implementation is wrong.

Good AI evaluation requires proportional judgement based on the actual requirements, technical risks and practical impact.
