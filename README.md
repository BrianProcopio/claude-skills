# Claude Skills

A collection of custom [Claude Code skills](https://docs.anthropic.com/en/docs/claude-code).

## Skills

### TypeScript Code Principles

Enforces 10 core coding principles when writing or reviewing TypeScript/JavaScript code:

1. Early returns over deep nesting
2. Clear, descriptive names
3. Single responsibility
4. Meaningful comments only
5. DRY for business logic
6. Fail fast and early
7. No magic values
8. Pure functions over side effects
9. Readable over clever
10. No premature optimization

Each principle includes bad vs good examples. See [typescript-code-principles/references/principles.md](typescript-code-principles/references/principles.md) for the full reference.

### JIRA Ticket Writer

Generates well-structured JIRA tickets from short descriptions. Produces a concise, searchable summary line and a structured description optimized for claude-code to consume as a task specification.

**Trigger phrases:** `/jira`, "write a ticket for...", "create a story for...", "I need a JIRA for..."

**Supported ticket types:** Story, Bug, Task, Epic

**Output includes:**
- **Summary** — action-oriented, max ~80 chars, optimized for search and scanning
- **Description** — structured with Context, Requirements, Acceptance Criteria, and Technical Notes sections

See [jira/SKILL.md](jira/SKILL.md) for the full template and guidance.

## Usage

Install a skill by copying its folder into your Claude Code skills directory, or reference it directly in your project configuration.

The skills activate automatically when writing or reviewing code, and can also be triggered explicitly by asking Claude to "review code", "check coding standards", or "apply code principles".
