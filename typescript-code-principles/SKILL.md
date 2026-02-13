---
name: typescript-code-principles
description: Organization coding principles and standards for TypeScript/JavaScript. Apply these principles when writing, reviewing, or refactoring any TS/JS code. Also use when explicitly asked to "review code", "check coding standards", "apply code principles", or "review for best practices". Enforces 10 core principles - early returns, clear naming, single responsibility, meaningful comments, DRY business logic, fail-fast, no magic values, pure functions, readability over cleverness, and no premature optimization.
---

# Code Principles

Apply these 10 principles to all TypeScript/JavaScript code — both when writing new code and when reviewing existing code.

## Principles Summary

1. **Early returns over deep nesting** — Use guard clauses; keep happy path at lowest indentation
2. **Clear, descriptive names** — Unambiguous names in plain English for functions, constants, variables
3. **Single responsibility** — Keep functions small and focused on one task
4. **Meaningful comments only** — Comment the "why", never the "what"
5. **DRY for business logic** — Extract repeated logic; duplicating simple lines is acceptable
6. **Fail fast and early** — Validate and throw at the top before doing work
7. **No magic values** — Name hardcoded numbers and strings with constants
8. **Pure functions over side effects** — Separate computation from state mutation
9. **Readable over clever** — Prioritize clarity and debuggability over brevity
10. **No premature optimization** — Use the simplest approach; optimize only when measured

For detailed examples of each principle (bad vs good patterns), see [references/principles.md](references/principles.md).

## When Writing Code

- Apply all 10 principles by default
- Use guard clauses and early returns to keep nesting shallow
- Name functions and variables descriptively — if a name needs a comment, rename it
- Extract logic into focused single-responsibility functions
- Define named constants for any non-obvious literal values

## When Reviewing Code

Check each principle against the code under review. Flag violations with the principle number and a concrete fix. Example:

> **Principle 7 violation**: `if (status === 3)` — extract `3` to a named constant like `const COMPLETED_STATUS = 3`
