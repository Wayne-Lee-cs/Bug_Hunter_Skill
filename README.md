# Bug Hunter 🐛🔍

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Version](https://img.shields.io/badge/version-1.1.0-blue.svg)](https://github.com/Wayne-Lee-cs/Bug_Hunter_Skill)

**Systematically hunt and detect potential bugs in your code before they reach production.**

A comprehensive bug detection prompt/instruction for AI coding assistants. Works with Claude Code, OpenAI Codex, Cursor, Qoder, Windsurf, and more.

## Features

- **8 Detection Categories**: Security vulnerabilities, input validation, null safety, exception handling, logic defects, code smells, concurrency issues, performance problems
- **Multi-Language Support**: Python, JavaScript/TypeScript, Java, Go, Rust, C/C++, PHP, and more
- **Severity Levels**: Critical 🔴, High 🟠, Medium 🟡, Low 🟢
- **Structured Reports**: Clear issue location, impact analysis, and fix suggestions
- **Actionable Checklists**: Copy-paste checklists for systematic code review

## Quick Start

### Claude Code

Copy the contents of `SKILL.md` (or `CLAUDE.md` for a condensed version) into your project's `CLAUDE.md`:

```markdown
# Bug Detection Instructions

When reviewing code for bugs, follow these guidelines:

[Paste SKILL.md content here]
```

### OpenAI Codex / Agents

Copy the contents of `AGENTS.md` into your project's `AGENTS.md`:

```markdown
# Bug Detection Agent

When hunting for bugs in code, follow this methodology:

[Paste AGENTS.md content here]
```

### Cursor

Copy the contents of `SKILL.md` into your `.cursorrules` file:

```markdown
# Bug Detection Rules

[Paste SKILL.md content here]
```

### Qoder

Place in `.qoder/skills/bug-hunter/` directory:

```
.qoder/skills/bug-hunter/
├── SKILL.md
├── CHECKLIST.md
└── examples.md
```

### Direct Use (Any Agent)

Simply paste the contents of `SKILL.md` into your conversation with any AI assistant:

```
Please analyze the following code for bugs using this methodology:

[Paste SKILL.md content]

Here's the code to analyze:
[Your code]
```

## File Structure

```
bug-hunter/
├── SKILL.md          # Core instructions - the main prompt
├── CHECKLIST.md      # Detailed checklists for systematic review
├── examples.md       # Real-world bug examples with fixes
├── README.md         # This file
└── LICENSE           # MIT License
```

## Detection Categories

### 1. Security Vulnerabilities
- SQL/Command injection
- Cross-site scripting (XSS)
- Authentication/Authorization flaws
- CSRF/SSRF

### 2. Input Validation
- Missing validation
- Unescaped output
- Path traversal

### 3. Null Safety & Boundary Issues
- Null/undefined dereference
- Array bounds checking
- Off-by-one errors
- Division by zero

### 4. Exception Handling Gaps
- Silent exception swallowing
- Resource leaks
- Missing try-catch blocks
- Unclosed resources

### 5. Logic Defects
- Unreachable code
- Incorrect boolean logic
- Missing break statements
- Infinite loops

### 6. Code Smells
- Magic numbers
- Deep nesting
- Duplicate code
- Hardcoded secrets

### 7. Concurrency Problems
- Race conditions
- Deadlocks
- Missing synchronization
- Async/await misuse

### 8. Performance Issues
- N+1 queries
- Memory leaks
- Inefficient algorithms
- Missing pagination

## Example Output

```markdown
## Bug Hunt Report

### Summary
- Critical: 2 issues
- High: 3 issues
- Medium: 5 issues
- Low: 2 issues

### Critical Issues 🔴

#### Null Pointer Dereference
**Location:** `user_service.py:45`
**Problem:** `user.email` accessed without null check
**Impact:** AttributeError when user is None
**Fix:**
​```python
if user is None:
    return None
return user.email
​```
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. Areas for contribution:

- Additional language-specific checks
- New bug detection patterns
- Improved examples
- Better documentation

## License

MIT License - feel free to use, modify, and distribute.

## Acknowledgments

Inspired by common bug patterns from:
- SonarQube rules
- ESLint best practices
- Google Code Review guidelines
- Microsoft Security Development Lifecycle
