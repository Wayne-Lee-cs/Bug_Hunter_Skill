# Bug Detection Checklist

Copy and track progress during code review:

```
## Review Progress
- [ ] 1. Security Vulnerabilities
- [ ] 2. Input Validation
- [ ] 3. Null Safety & Boundaries
- [ ] 4. Exception Handling  
- [ ] 5. Logic Defects
- [ ] 6. Code Smells
- [ ] 7. Concurrency Issues
- [ ] 8. Performance Issues
```

---

## 1. Security Vulnerabilities

### Injection Attacks
- [ ] SQL queries use parameterized statements
- [ ] Command execution doesn't include user input
- [ ] LDAP queries are properly escaped
- [ ] XPath queries are parameterized

### Cross-Site Scripting (XSS)
- [ ] User input is HTML-encoded before output
- [ ] `innerHTML` / `dangerouslySetInnerHTML` avoided or sanitized
- [ ] Content-Security-Policy headers configured
- [ ] DOM manipulation uses safe methods

### Authentication & Authorization
- [ ] Passwords are hashed with strong algorithms (bcrypt, Argon2)
- [ ] Rate limiting on authentication endpoints
- [ ] Session tokens are secure and HttpOnly
- [ ] Access control checks on every protected endpoint
- [ ] No IDOR (Insecure Direct Object References)

### Data Exposure
- [ ] No secrets/credentials in code or logs
- [ ] PII is encrypted at rest and in transit
- [ ] Error messages don't leak internal details
- [ ] Debug endpoints disabled in production

### CSRF & SSRF
- [ ] CSRF tokens on state-changing operations
- [ ] URL validation for server-side requests
- [ ] Whitelist for external service calls

---

## 2. Input Validation & Sanitization

### Validation
- [ ] All user inputs are validated
- [ ] Length/size limits enforced
- [ ] Format validation (email, phone, etc.)
- [ ] Range checks for numeric values
- [ ] Whitelist validation where possible

### Sanitization
- [ ] Output encoded for context (HTML, SQL, Shell)
- [ ] File uploads validated (type, size, content)
- [ ] Path traversal characters blocked
- [ ] Regex patterns are not vulnerable to ReDoS
- [ ] Deserialization only from trusted sources

---

## 3. Null Safety & Boundary Checks

### Null/Undefined Handling
- [ ] All nullable parameters are checked before use
- [ ] Optional return values are handled properly
- [ ] Nullable fields in objects are guarded
- [ ] No chained method calls on potentially null objects

### Array/Collection Boundaries
- [ ] Array index access is bounds-checked
- [ ] Collections are checked for empty before first/last access
- [ ] Substring operations have valid indices
- [ ] Loop indices don't exceed collection size

### Numeric Boundaries
- [ ] No division by zero possibility
- [ ] Integer overflow/underflow is handled
- [ ] Floating point comparisons use epsilon
- [ ] Negative numbers handled where unsigned expected

---

## 4. Exception Handling

### Resource Management
- [ ] Files are closed in finally/using/defer
- [ ] Database connections are properly released
- [ ] Network sockets are closed on error
- [ ] Locks are released in all code paths

### Error Handling Quality
- [ ] No empty catch blocks
- [ ] Exceptions are logged with context
- [ ] Specific exceptions are caught (not generic Exception)
- [ ] Errors are propagated or handled, not ignored

### Cleanup Safety
- [ ] Finally blocks don't throw
- [ ] Cleanup code handles partial initialization
- [ ] Resources cleaned up in reverse order of acquisition

---

## 5. Logic Defects

### Boolean Logic
- [ ] De Morgan's law applied correctly
- [ ] Short-circuit evaluation used properly
- [ ] No always-true/always-false conditions
- [ ] XOR vs OR vs AND used correctly

### Control Flow
- [ ] All switch cases have break or explicit fallthrough
- [ ] Loops have proper exit conditions
- [ ] Recursive functions have base cases
- [ ] Return statements reach correctly

### Comparisons
- [ ] == vs = used correctly (assignment vs comparison)
- [ ] === used for strict equality (JS/TS)
- [ ] Floating point compared with tolerance
- [ ] String comparison considers null/case

### Return Values
- [ ] All code paths return a value
- [ ] Error return values are checked
- [ ] Promise/Future results are awaited

---

## 6. Code Smells

### Maintainability
- [ ] No functions longer than 50 lines
- [ ] No deeply nested code (>4 levels)
- [ ] No duplicate code blocks (DRY)
- [ ] Single responsibility per function

### Magic Values
- [ ] No hardcoded numbers (use constants)
- [ ] No hardcoded strings for keys/configs
- [ ] No hardcoded URLs or paths
- [ ] No hardcoded credentials

### Dead Code
- [ ] No unused variables
- [ ] No unused imports
- [ ] No unreachable code
- [ ] No commented-out code blocks

### Technical Debt
- [ ] TODO/FIXME comments addressed
- [ ] Deprecated API usage updated
- [ ] Warnings resolved

---

## 7. Concurrency Issues

### Shared State
- [ ] Mutable shared state is synchronized
- [ ] Collections use thread-safe variants
- [ ] Check-then-act sequences are atomic
- [ ] Lazy initialization is thread-safe

### Locks & Synchronization  
- [ ] Locks acquired in consistent order
- [ ] Locks released in all code paths
- [ ] No nested locks that could deadlock
- [ ] Lock scope is minimal

### Async/Await Patterns
- [ ] All promises are awaited or intentionally fire-and-forget
- [ ] No async void except event handlers
- [ ] Cancellation tokens are respected
- [ ] Parallel operations properly joined

### Thread Safety
- [ ] Singletons are thread-safe
- [ ] Static mutable state is protected
- [ ] Event handlers don't cause races
- [ ] Object publication is safe

---

## 8. Performance Issues

### Database
- [ ] No N+1 query patterns
- [ ] Large result sets are paginated
- [ ] Indexes exist for frequent queries
- [ ] Connection pooling configured

### Memory & CPU
- [ ] No unbounded caches or collections
- [ ] Large objects are streamed, not buffered
- [ ] Expensive computations are cached
- [ ] No blocking I/O in async contexts

### Algorithms
- [ ] Time complexity is appropriate
- [ ] No unnecessary nested loops
- [ ] Early exit conditions where possible
- [ ] Batch operations instead of individual calls

---

## Language-Specific Checklists

### Python
- [ ] Mutable default arguments avoided
- [ ] Late binding in closures handled
- [ ] `with` statement used for resources
- [ ] `isinstance()` instead of `type()` comparison
- [ ] No `eval()`/`exec()` on user input
- [ ] No `pickle.loads()` on untrusted data

### JavaScript/TypeScript
- [ ] `===` used instead of `==`
- [ ] `let`/`const` instead of `var`
- [ ] Callbacks handle errors
- [ ] Event listeners removed when component unmounts
- [ ] No `eval()` or `Function()` on user input
- [ ] `textContent` used instead of `innerHTML` where possible

### Java
- [ ] `equals()` and `hashCode()` consistent
- [ ] `compareTo()` consistent with `equals()`
- [ ] Try-with-resources used
- [ ] Checked exceptions handled
- [ ] PreparedStatement used for SQL
- [ ] No ObjectInputStream on untrusted data

### Go
- [ ] Error returns checked
- [ ] Goroutines don't leak (use WaitGroup/context)
- [ ] Context cancellation respected
- [ ] Maps not accessed concurrently without sync
- [ ] No `fmt.Sprintf` in SQL queries

### Rust
- [ ] `unwrap()` usage justified
- [ ] Error propagation with `?` operator
- [ ] Lifetimes explicit where needed
- [ ] `unsafe` blocks documented and minimized

### C/C++
- [ ] Buffer sizes checked before copy
- [ ] Memory freed exactly once
- [ ] Null checks before pointer dereference
- [ ] Integer operations checked for overflow
- [ ] Format strings don't include user input

### PHP
- [ ] PDO with prepared statements used
- [ ] No `include`/`require` with user input
- [ ] No `unserialize()` on untrusted data
- [ ] CSRF tokens validated
- [ ] File upload type/content validated
