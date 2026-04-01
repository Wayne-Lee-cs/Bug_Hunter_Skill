# Bug Hunter Examples

Real-world examples of bugs detected using this skill.

---

## Example 1: SQL Injection

### Buggy Code (Python)
```python
def get_user(username):
    query = f"SELECT * FROM users WHERE username = '{username}'"
    return db.execute(query)  # 🔴 SQL Injection!
```

### Bug Report
**Severity:** 🔴 Critical  
**Category:** Security / Injection  
**Problem:** User input directly concatenated into SQL query  
**Impact:** Attacker can inject `'; DROP TABLE users; --` to delete data or extract sensitive information

### Fixed Code
```python
def get_user(username):
    query = "SELECT * FROM users WHERE username = ?"
    return db.execute(query, (username,))  # Parameterized query
```

---

## Example 2: Cross-Site Scripting (XSS)

### Buggy Code (JavaScript/React)
```javascript
function Comment({ text }) {
    return <div dangerouslySetInnerHTML={{ __html: text }} />;  // 🔴 XSS!
}

// Or vanilla JS:
element.innerHTML = userInput;  // 🔴 XSS!
```

### Bug Report
**Severity:** 🔴 Critical  
**Category:** Security / XSS  
**Problem:** User-controlled content rendered as HTML without sanitization  
**Impact:** Attacker can inject `<script>stealCookies()</script>` to steal user sessions

### Fixed Code
```javascript
import DOMPurify from 'dompurify';

function Comment({ text }) {
    // Option 1: Sanitize HTML
    const clean = DOMPurify.sanitize(text);
    return <div dangerouslySetInnerHTML={{ __html: clean }} />;
    
    // Option 2: Use textContent (preferred if HTML not needed)
    return <div>{text}</div>;  // React auto-escapes
}

// Vanilla JS:
element.textContent = userInput;  // Safe - treats as text
```

---

## Example 3: Null Pointer Dereference

### Buggy Code (Python)
```python
def get_user_email(user_id):
    user = database.find_user(user_id)
    return user.email  # 🔴 Bug: user could be None
```

### Bug Report
**Severity:** 🔴 Critical  
**Category:** Null Safety  
**Problem:** `find_user()` returns `None` if user not found, but code accesses `.email` without null check  
**Impact:** `AttributeError: 'NoneType' object has no attribute 'email'`

### Fixed Code
```python
def get_user_email(user_id):
    user = database.find_user(user_id)
    if user is None:
        return None  # or raise UserNotFoundError
    return user.email
```

---

## Example 4: Off-By-One Error

### Buggy Code (JavaScript)
```javascript
function getLastItems(arr, n) {
    const result = [];
    for (let i = arr.length - n; i <= arr.length; i++) {
        result.push(arr[i]);  // 🔴 Bug: i <= arr.length causes undefined
    }
    return result;
}
// Also: if n > arr.length, starts from negative index!
```

### Bug Report
**Severity:** 🔴 Critical  
**Category:** Boundary Issue  
**Problem:** Loop uses `<=` instead of `<`, accessing `arr[arr.length]` which is undefined. Also, no validation when `n > arr.length`  
**Impact:** Returns array with `undefined` as last element, or accesses negative indices

### Fixed Code
```javascript
function getLastItems(arr, n) {
    if (!arr || arr.length === 0) return [];
    const count = Math.min(n, arr.length);  // Prevent negative index
    const result = [];
    for (let i = arr.length - count; i < arr.length; i++) {
        result.push(arr[i]);
    }
    return result;
}

// Or more elegantly:
function getLastItemsClean(arr, n) {
    if (!arr) return [];
    return arr.slice(-Math.min(n, arr.length));
}
```

---

## Example 5: Resource Leak

### Buggy Code (Java)
```java
public String readFile(String path) throws IOException {
    FileReader reader = new FileReader(path);
    BufferedReader br = new BufferedReader(reader);
    StringBuilder sb = new StringBuilder();
    String line;
    while ((line = br.readLine()) != null) {
        sb.append(line);
    }
    // 🔴 Bug: reader and br never closed
    return sb.toString();
}
```

### Bug Report
**Severity:** 🟠 High  
**Category:** Exception Handling / Resource Management  
**Problem:** FileReader and BufferedReader are never closed  
**Impact:** File handle leak, eventually causes "Too many open files" error

### Fixed Code
```java
public String readFile(String path) throws IOException {
    try (BufferedReader br = new BufferedReader(new FileReader(path))) {
        StringBuilder sb = new StringBuilder();
        String line;
        while ((line = br.readLine()) != null) {
            sb.append(line);
        }
        return sb.toString();
    }
}
```

---

## Example 6: Race Condition

### Buggy Code (Go)
```go
var counter int

func incrementCounter() {
    for i := 0; i < 1000; i++ {
        counter++  // 🔴 Bug: not atomic
    }
}

func main() {
    go incrementCounter()
    go incrementCounter()
    time.Sleep(time.Second)  // 🟠 Also bad: using Sleep instead of WaitGroup
    fmt.Println(counter)  // Expected 2000, but result is unpredictable
}
```

### Bug Report
**Severity:** 🔴 Critical  
**Category:** Concurrency / Race Condition  
**Problem:** Multiple goroutines increment shared variable without synchronization, and uses `time.Sleep` instead of proper synchronization  
**Impact:** Data race causes incorrect count, undefined behavior

### Fixed Code
```go
import (
    "fmt"
    "sync"
    "sync/atomic"
)

var counter int64

func incrementCounter(wg *sync.WaitGroup) {
    defer wg.Done()
    for i := 0; i < 1000; i++ {
        atomic.AddInt64(&counter, 1)
    }
}

func main() {
    var wg sync.WaitGroup
    wg.Add(2)
    go incrementCounter(&wg)
    go incrementCounter(&wg)
    wg.Wait()  // Proper synchronization
    fmt.Println(counter)  // Now reliably prints 2000
}
```

---

## Example 7: Silent Exception Swallowing

### Buggy Code (TypeScript)
```typescript
async function fetchUserData(userId: string) {
    try {
        const response = await api.get(`/users/${userId}`);
        return response.data;
    } catch (error) {
        // 🟠 Bug: error silently swallowed
        return null;
    }
}
```

### Bug Report
**Severity:** 🟠 High  
**Category:** Exception Handling  
**Problem:** Catch block returns null without logging, making debugging impossible  
**Impact:** Errors go unnoticed, hard to diagnose production issues

### Fixed Code
```typescript
async function fetchUserData(userId: string) {
    try {
        const response = await api.get(`/users/${userId}`);
        return response.data;
    } catch (error) {
        logger.error(`Failed to fetch user ${userId}:`, error);
        throw new UserFetchError(userId, error);
    }
}
```

---

## Example 8: Mutable Default Argument (Python-specific)

### Buggy Code
```python
def add_item(item, items=[]):  # 🔴 Bug: mutable default
    items.append(item)
    return items

# First call
print(add_item("a"))  # ['a']
# Second call - unexpected!
print(add_item("b"))  # ['a', 'b'] - not ['b']!
```

### Bug Report
**Severity:** 🔴 Critical  
**Category:** Language-Specific / Logic Defect  
**Problem:** Mutable default argument `[]` is shared across all calls  
**Impact:** List accumulates items from all calls, causing data leakage

### Fixed Code
```python
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

---

## Example 9: Missing Await

### Buggy Code (JavaScript)
```javascript
async function processOrders(orderIds) {
    const results = [];
    for (const id of orderIds) {
        results.push(processOrder(id));  // 🔴 Bug: missing await
    }
    return results;  // Returns array of Promises, not results
}
```

### Bug Report
**Severity:** 🔴 Critical  
**Category:** Async/Await Misuse  
**Problem:** `await` missing before async function call  
**Impact:** Returns unresolved Promises instead of actual results

### Fixed Code
```javascript
async function processOrders(orderIds) {
    const results = [];
    for (const id of orderIds) {
        results.push(await processOrder(id));
    }
    return results;
}

// Or for parallel processing:
async function processOrdersParallel(orderIds) {
    return Promise.all(orderIds.map(id => processOrder(id)));
}
```

---

## Example 10: Division By Zero

### Buggy Code (C#)
```csharp
public double CalculateAverage(int[] numbers) {
    int sum = 0;
    foreach (var n in numbers) {
        sum += n;  // 🟠 Potential integer overflow for large arrays
    }
    return sum / numbers.Length;  // 🔴 Bug: division by zero if empty
                                  // 🟠 Bug: integer division loses precision
}
```

### Bug Report
**Severity:** 🔴 Critical  
**Category:** Boundary Issue  
**Problem:** No check for empty/null array, integer division loses decimal precision, potential overflow  
**Impact:** `DivideByZeroException` when array is empty, incorrect results for non-integer averages

### Fixed Code
```csharp
public double CalculateAverage(int[] numbers) {
    if (numbers == null || numbers.Length == 0) {
        throw new ArgumentException("Array must not be null or empty", nameof(numbers));
    }
    // Use long to prevent overflow, or LINQ for cleaner code
    long sum = 0;
    foreach (var n in numbers) {
        sum += n;
    }
    return (double)sum / numbers.Length;
}

// Or using LINQ (recommended):
public double CalculateAverageLinq(int[] numbers) {
    if (numbers == null || numbers.Length == 0) {
        throw new ArgumentException("Array must not be null or empty", nameof(numbers));
    }
    return numbers.Average();  // Handles precision correctly
}
```
