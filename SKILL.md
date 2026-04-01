# Bug Hunter

> Systematically hunt and detect potential bugs in code including null safety issues, boundary conditions, exception handling gaps, logic defects, code smells, and concurrency problems.

A comprehensive bug detection prompt/instruction that systematically analyzes code to find potential issues before they reach production. Works with Claude Code, OpenAI Codex, Cursor, Qoder, and other AI coding assistants.

## Quick Start

When hunting for bugs:

1. **Scan the target code** - Read and understand the code structure
2. **Apply detection checklist** - Go through each category systematically
3. **Report findings** - Organize issues by severity and category
4. **Suggest fixes** - Provide actionable remediation guidance

## Detection Categories

### 1. Null Safety & Boundary Issues

Check for:
- Null/undefined dereference without guards
- Array/collection access without bounds checking
- Off-by-one errors in loops
- Empty collection handling
- Division by zero possibilities

### 2. Exception Handling Gaps

Check for:
- Catch blocks that swallow exceptions silently
- Missing try-catch for operations that can throw
- Unclosed resources (files, connections, streams)
- Finally blocks with return statements
- Exception type too broad (catching `Exception` instead of specific types)

### 3. Logic Defects

Check for:
- Unreachable code paths
- Incorrect boolean logic (De Morgan's law violations)
- Wrong comparison operators (= vs ==, < vs <=)
- Missing break in switch/case
- Infinite loop possibilities
- Return value ignored when it shouldn't be

### 4. Code Smells

Check for:
- Duplicate code blocks
- Magic numbers without constants
- Functions/methods that are too long (>50 lines)
- Deep nesting (>4 levels)
- Unused variables or imports
- Hardcoded credentials or secrets
- TODO/FIXME comments that indicate known issues

### 5. Concurrency Problems

Check for:
- Race conditions on shared mutable state
- Deadlock possibilities (lock ordering)
- Non-atomic operations assumed to be atomic
- Missing synchronization
- Thread-unsafe singleton implementations
- Async/await misuse (missing await, fire-and-forget)

## Severity Levels

Report issues using these levels:

| Level | Icon | Description |
|-------|------|-------------|
| **Critical** | 🔴 | Will cause crashes or data corruption |
| **High** | 🟠 | Likely to cause bugs in edge cases |
| **Medium** | 🟡 | Code smell that may lead to issues |
| **Low** | 🟢 | Minor improvement suggestion |

## Output Format

```markdown
## Bug Hunt Report

### Summary
- Critical: X issues
- High: X issues  
- Medium: X issues
- Low: X issues

### Critical Issues 🔴

#### [Issue Title]
**Location:** `file.py:123`
**Problem:** Description of the issue
**Impact:** What can go wrong
**Fix:** 
```code
// suggested fix
```

### High Issues 🟠
...
```

## Language-Specific Checks

### Python
- Missing `if __name__ == "__main__"` guard
- Mutable default arguments
- Late binding closures in loops
- String formatting vulnerabilities

### JavaScript/TypeScript
- `==` instead of `===`
- Missing `async/await` handling
- Prototype pollution risks
- Memory leaks in closures/event listeners

### Java/Kotlin
- Unchecked casts
- Resource leaks (try-with-resources missing)
- Null checks after method calls that can't return null
- Serialization vulnerabilities

### Go
- Ignored error returns
- Goroutine leaks
- Data races on maps
- Deferred function argument evaluation

### Rust
- Unwrap on Result/Option without handling
- Integer overflow in release mode
- Unsafe blocks without proper documentation

## Additional Resources

- For detailed checklists, see [CHECKLIST.md](CHECKLIST.md)
- For real-world examples, see [examples.md](examples.md)
