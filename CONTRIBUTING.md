# Contributing to Bug Hunter

Thank you for your interest in contributing to Bug Hunter! This project aims to help developers detect bugs before they reach production.

## How to Contribute

### Reporting Issues

- Check existing issues before creating a new one
- Use descriptive titles and provide detailed descriptions
- Include examples when reporting false positives/negatives

### Suggesting New Detection Patterns

1. **Fork** the repository
2. **Add** your detection pattern to the appropriate section in `SKILL.md`
3. **Add** a corresponding checklist item to `CHECKLIST.md`
4. **Include** a real-world example in `examples.md` with:
   - Buggy code
   - Bug report (severity, category, problem, impact)
   - Fixed code
5. **Submit** a Pull Request

### Detection Pattern Guidelines

When adding new patterns, ensure they:

- **Are common**: Affect many codebases, not edge cases
- **Are actionable**: Clear how to fix the issue
- **Have low false positives**: Don't flag correct code
- **Include severity**: Critical, High, Medium, or Low
- **Are language-aware**: Note if pattern is language-specific

### Code Examples Guidelines

Good examples should:

- Be **minimal**: Show only the relevant code
- Be **realistic**: Represent actual bugs developers make
- Include **comments**: Mark the bug location with emoji (🔴, 🟠)
- Show **correct fix**: Not just identify the problem

### Pull Request Process

1. Update documentation if you change functionality
2. Keep PRs focused - one feature/fix per PR
3. Follow existing formatting and style
4. Test that markdown renders correctly

## Areas We Need Help

- [ ] Additional language-specific checks (Swift, Kotlin, Scala)
- [ ] More security vulnerability patterns
- [ ] Performance anti-patterns
- [ ] Framework-specific checks (React, Django, Spring)
- [ ] Translations of documentation

## Code of Conduct

- Be respectful and inclusive
- Focus on constructive feedback
- Help others learn and grow

## Questions?

Open an issue with the `question` label or start a discussion.

---

Thank you for helping make Bug Hunter better! 🐛🔍
