# AI Code Evaluation Portfolio

A collection of structured technical evaluations of software and AI-generated code.

This repository demonstrates my approach to reviewing technical solutions for correctness, security, asynchronous behaviour, type safety, maintainability and production readiness.

## About

AI-generated code can appear correct while still containing serious problems.

A solution may satisfy the happy path but fail because of:

- Security vulnerabilities
- Race conditions
- Stale asynchronous state
- Missing error handling
- Weak type safety
- Incorrect assumptions
- Edge-case failures
- Poor production readiness

The purpose of this repository is to demonstrate structured technical judgement when identifying and evaluating those issues.

## Evaluation Methodology

Each evaluation follows a consistent process:

1. Understand the original requirement.
2. Analyse the proposed implementation.
3. Identify functional and non-functional issues.
4. Trace realistic failure scenarios.
5. Assess the severity and impact of each issue.
6. Recommend an improved implementation where appropriate.
7. Provide a justified production verdict.

## Evaluation Areas

### 01 — SQL Injection Vulnerability

Review of backend code containing direct interpolation of user-controlled input into an SQL query.

Focus areas:

- SQL injection
- Database result handling
- Parameterized queries
- Production security

### 02 — React Async Race Conditions

Evaluation of a React data-fetching implementation that can display stale data when requests complete out of order.

Focus areas:

- useEffect
- Asynchronous operations
- Race conditions
- Stale state
- AbortController
- Production reliability

### 03 — TypeScript Type Safety

Evaluation of the risks of using `any` when a clear object structure can be defined.

Focus areas:

- Type safety
- Compile-time validation
- Interfaces
- `any` versus `unknown`
- Maintainability

### 04 — Authentication and Browser Security

Evaluation of common misconceptions around JWTs, localStorage and HttpOnly cookies.

Focus areas:

- JWT security
- XSS
- CSRF
- HttpOnly cookies
- Secure cookies
- SameSite
- Security trade-offs

### 05 — AI Model Comparison

A structured comparison of two AI-generated React implementations.

Focus areas:

- Requirement compliance
- Functional correctness
- State update patterns
- Proportional technical judgement
- Production readiness

## Evaluation Principles

A better implementation does not automatically mean that another implementation is incorrect.

My evaluations distinguish between:

- Critical failures
- Major issues
- Minor issues
- Design trade-offs
- Correct implementations with limitations
- Correct implementations that follow stronger engineering practices

The goal is to provide proportional and evidence-based technical judgement rather than treating every stylistic difference as a failure.

## Background

I am a frontend-focused Software Engineer with experience working with:

- React
- TypeScript
- JavaScript
- Electron
- REST APIs
- WebSockets
- Node.js
- Express
- MySQL and PostgreSQL

My engineering work has involved debugging production issues, improving application performance, refactoring legacy systems, working with asynchronous application flows and building web and desktop applications.

## Repository Structure

    evaluations/
    ├── 01-sql-injection-review.md
    ├── 02-react-async-race-conditions.md
    ├── 03-typescript-type-safety.md
    ├── 04-authentication-security.md
    └── 05-ai-model-comparison.md

## Purpose

This repository is a practical portfolio demonstrating how I approach technical review and AI-generated code evaluation.

The evaluations are independent portfolio exercises and are not presented as work performed for any AI company or client.
