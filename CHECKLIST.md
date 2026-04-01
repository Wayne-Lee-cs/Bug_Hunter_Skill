# Bug Detection Checklist

Copy and track progress during code review:

```
## Review Progress
- [ ] 1. Null Safety & Boundaries
- [ ] 2. Exception Handling  
- [ ] 3. Logic Defects
- [ ] 4. Code Smells
- [ ] 5. Concurrency Issues
```

---

## 1. Null Safety & Boundary Checks

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

## 2. Exception Handling

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

## 3. Logic Defects

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

## 4. Code Smells

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

## 5. Concurrency Issues

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

## Language-Specific Checklists

### Python
- [ ] Mutable default arguments avoided
- [ ] Late binding in closures handled
- [ ] `with` statement used for resources
- [ ] `isinstance()` instead of `type()` comparison

### JavaScript/TypeScript
- [ ] `===` used instead of `==`
- [ ] `let`/`const` instead of `var`
- [ ] Callbacks handle errors
- [ ] Event listeners removed when component unmounts

### Java
- [ ] `equals()` and `hashCode()` consistent
- [ ] `compareTo()` consistent with `equals()`
- [ ] Try-with-resources used
- [ ] Checked exceptions handled

### Go
- [ ] Error returns checked
- [ ] Goroutines don't leak
- [ ] Context cancellation respected
- [ ] Maps not accessed concurrently without sync

### Rust
- [ ] `unwrap()` usage justified
- [ ] Error propagation with `?` operator
- [ ] Lifetimes explicit where needed
- [ ] `unsafe` blocks documented
