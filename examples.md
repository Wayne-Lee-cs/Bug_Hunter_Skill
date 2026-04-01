# Bug Hunter Examples

Real-world examples of bugs detected using this skill.

---

## Example 1: Null Pointer Dereference

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

## Example 2: Off-By-One Error

### Buggy Code (JavaScript)
```javascript
function getLastItems(arr, n) {
    const result = [];
    for (let i = arr.length - n; i <= arr.length; i++) {
        result.push(arr[i]);  // 🔴 Bug: i <= arr.length causes undefined
    }
    return result;
}
```

### Bug Report
**Severity:** 🔴 Critical  
**Category:** Boundary Issue  
**Problem:** Loop uses `<=` instead of `<`, accessing `arr[arr.length]` which is undefined  
**Impact:** Returns array with `undefined` as last element

### Fixed Code
```javascript
function getLastItems(arr, n) {
    const result = [];
    for (let i = arr.length - n; i < arr.length; i++) {
        result.push(arr[i]);
    }
    return result;
}
```

---

## Example 3: Resource Leak

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

## Example 4: Race Condition

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
    time.Sleep(time.Second)
    fmt.Println(counter)  // Expected 2000, but result is unpredictable
}
```

### Bug Report
**Severity:** 🔴 Critical  
**Category:** Concurrency / Race Condition  
**Problem:** Multiple goroutines increment shared variable without synchronization  
**Impact:** Data race causes incorrect count, undefined behavior

### Fixed Code
```go
var counter int64

func incrementCounter() {
    for i := 0; i < 1000; i++ {
        atomic.AddInt64(&counter, 1)
    }
}
```

---

## Example 5: Silent Exception Swallowing

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

## Example 6: Mutable Default Argument (Python-specific)

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

## Example 7: Missing Await

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

## Example 8: Division By Zero

### Buggy Code (C#)
```csharp
public double CalculateAverage(int[] numbers) {
    int sum = 0;
    foreach (var n in numbers) {
        sum += n;
    }
    return sum / numbers.Length;  // 🔴 Bug: division by zero if empty
}
```

### Bug Report
**Severity:** 🔴 Critical  
**Category:** Boundary Issue  
**Problem:** No check for empty array before division  
**Impact:** `DivideByZeroException` when array is empty

### Fixed Code
```csharp
public double CalculateAverage(int[] numbers) {
    if (numbers == null || numbers.Length == 0) {
        return 0;  // or throw ArgumentException
    }
    int sum = 0;
    foreach (var n in numbers) {
        sum += n;
    }
    return (double)sum / numbers.Length;
}
```
