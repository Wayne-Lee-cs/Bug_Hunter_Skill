# Bug Hunter - Claude Code Integration

This file shows how to integrate Bug Hunter with Claude Code projects.

## Setup

Copy the following into your project's `CLAUDE.md` file:

---

## Bug Detection Guidelines

When reviewing code for bugs or when the user asks to "hunt bugs", "find issues", or "review code quality", follow this systematic approach:

### Detection Process

1. **Scan** - Read and understand the code structure
2. **Check** - Apply detection checklist systematically
3. **Report** - Organize issues by severity
4. **Fix** - Provide actionable remediation

### Detection Categories

#### 1. Null Safety & Boundary Issues
- Null/undefined dereference without guards
- Array/collection access without bounds checking
- Off-by-one errors in loops
- Division by zero possibilities

#### 2. Exception Handling Gaps
- Catch blocks that swallow exceptions silently
- Missing try-catch for operations that can throw
- Unclosed resources (files, connections, streams)
- Exception type too broad

#### 3. Logic Defects
- Unreachable code paths
- Incorrect boolean logic
- Wrong comparison operators (= vs ==, < vs <=)
- Missing break in switch/case
- Return value ignored when it shouldn't be

#### 4. Code Smells
- Magic numbers without constants
- Functions >50 lines / Deep nesting >4 levels
- Hardcoded credentials or secrets
- TODO/FIXME indicating known issues

#### 5. Concurrency Problems
- Race conditions on shared mutable state
- Deadlock possibilities
- Missing synchronization
- Async/await misuse (missing await)

### Severity Levels

| Level | Icon | Description |
|-------|------|-------------|
| Critical | 🔴 | Will cause crashes or data corruption |
| High | 🟠 | Likely to cause bugs in edge cases |
| Medium | 🟡 | Code smell that may lead to issues |
| Low | 🟢 | Minor improvement suggestion |

### Report Format

```markdown
## Bug Hunt Report

### Summary
- Critical: X issues
- High: X issues

### Critical Issues 🔴

#### [Issue Title]
**Location:** `file:line`
**Problem:** Description
**Impact:** What can go wrong
**Fix:** Code suggestion
```

### Language-Specific Checks

**Python**: Mutable default arguments, late binding closures
**JavaScript/TypeScript**: `==` vs `===`, missing await
**Java**: Resource leaks, unchecked casts
**Go**: Ignored error returns, goroutine leaks
**Rust**: Unwrap without handling, unsafe blocks
