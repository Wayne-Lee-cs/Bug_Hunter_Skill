# Bug Hunter

> **v1.1.0** | Systematically hunt and detect potential bugs in code including security vulnerabilities, null safety issues, boundary conditions, exception handling gaps, logic defects, code smells, and concurrency problems.

A comprehensive bug detection prompt/instruction that systematically analyzes code to find potential issues before they reach production. Works with Claude Code, OpenAI Codex, Cursor, Qoder, Windsurf, and other AI coding assistants.

## Quick Start

When hunting for bugs:

1. **Scan the target code** - Read and understand the code structure
2. **Apply detection checklist** - Go through each category systematically
3. **Report findings** - Organize issues by severity and category
4. **Suggest fixes** - Provide actionable remediation guidance

## Detection Categories

### 1. Security Vulnerabilities

Check for:
- **Injection**: SQL injection, command injection, LDAP injection
- **XSS**: Reflected, stored, DOM-based cross-site scripting
- **Authentication**: Weak passwords, missing rate limiting, session fixation
- **Authorization**: Broken access control, IDOR, privilege escalation
- **Data Exposure**: Sensitive data in logs, hardcoded secrets, PII leaks
- **Cryptography**: Weak algorithms, improper key management
- **SSRF/CSRF**: Server-side request forgery, cross-site request forgery
- **Path Traversal**: Directory traversal, file inclusion vulnerabilities

### 2. Input Validation & Sanitization

Check for:
- Missing input validation on user data
- Unescaped output in HTML/SQL/Shell contexts
- Missing length/format/range checks
- Type coercion vulnerabilities
- Regex DoS (ReDoS) patterns
- Deserialization of untrusted data

### 3. Null Safety & Boundary Issues

Check for:
- Null/undefined dereference without guards
- Array/collection access without bounds checking
- Off-by-one errors in loops
- Empty collection handling
- Division by zero possibilities
- Integer overflow/underflow

### 4. Exception Handling Gaps

Check for:
- Catch blocks that swallow exceptions silently
- Missing try-catch for operations that can throw
- Unclosed resources (files, connections, streams)
- Finally blocks with return statements
- Exception type too broad (catching `Exception` instead of specific types)

### 5. Logic Defects

Check for:
- Unreachable code paths
- Incorrect boolean logic (De Morgan's law violations)
- Wrong comparison operators (= vs ==, < vs <=)
- Missing break in switch/case
- Infinite loop possibilities
- Return value ignored when it shouldn't be

### 6. Code Smells

Check for:
- Duplicate code blocks
- Magic numbers without constants
- Functions/methods that are too long (>50 lines)
- Deep nesting (>4 levels)
- Unused variables or imports
- Hardcoded credentials or secrets
- TODO/FIXME comments that indicate known issues

### 7. Concurrency Problems

Check for:
- Race conditions on shared mutable state
- Deadlock possibilities (lock ordering)
- Non-atomic operations assumed to be atomic
- Missing synchronization
- Thread-unsafe singleton implementations
- Async/await misuse (missing await, fire-and-forget)

### 8. Performance Issues

Check for:
- N+1 database queries
- Missing pagination on large datasets
- Unbounded memory growth / memory leaks
- Inefficient algorithms (O(n²) when O(n) possible)
- Blocking I/O in async contexts
- Missing caching for expensive operations
- Unnecessary object creation in loops

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
- String formatting vulnerabilities (f-string injection)
- Pickle deserialization of untrusted data
- `eval()`/`exec()` on user input

### JavaScript/TypeScript
- `==` instead of `===`
- Missing `async/await` handling
- Prototype pollution risks
- Memory leaks in closures/event listeners
- `innerHTML` with unsanitized content
- `eval()`/`Function()` on user input

### Java/Kotlin
- Unchecked casts
- Resource leaks (try-with-resources missing)
- Null checks after method calls that can't return null
- Serialization/deserialization vulnerabilities
- SQL concatenation instead of prepared statements

### Go
- Ignored error returns
- Goroutine leaks (missing WaitGroup/context)
- Data races on maps
- Deferred function argument evaluation
- `fmt.Sprintf` in SQL queries

### Rust
- Unwrap on Result/Option without handling
- Integer overflow in release mode
- Unsafe blocks without proper documentation
- Use-after-free in unsafe code

### C/C++
- Buffer overflows
- Use-after-free / double-free
- Null pointer dereference
- Integer overflow
- Format string vulnerabilities
- Memory leaks (missing free)

### PHP
- SQL injection (missing PDO/prepared statements)
- `include`/`require` with user input
- Unserialize on untrusted data
- Missing CSRF tokens
- File upload vulnerabilities

## Additional Resources

- For detailed checklists, see [CHECKLIST.md](CHECKLIST.md)
- For real-world examples, see [examples.md](examples.md)
