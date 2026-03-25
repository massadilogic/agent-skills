---
name: clean-code-principles
description: >
  Enforces practical clean code and professional coding practices.
  Trigger: When writing, reviewing, or refactoring code in any language.
license: Apache-2.0
metadata:
  author: author-name
  version: "1.0"
  scope: [root]
  auto_invoke:
    - "Applying clean code principles"
allowed-tools: Read, Edit, Write, Glob, Grep, Bash, WebFetch, WebSearch, Task
---

## When to Use

- Writing new code (feature development)
- Refactoring existing code
- Performing code reviews
- Designing APIs or modules
- Adding tests or improving quality

---

## Critical Patterns

### 1. Functions Must Be Small and Focused
- One responsibility per function
- Prefer < 20 lines
- Extract aggressively

### 2. Naming Is a First-Class Concern
- Use intention-revealing names
- Avoid abbreviations and ambiguity
- Functions = verbs, variables = nouns

### 3. No Silent Failures
- Never ignore errors
- Fail fast or handle explicitly
- Avoid empty catch blocks

### 4. Tests Are Not Optional
- Code without tests is incomplete
- Prefer unit tests with fast execution
- Cover critical paths first

### 5. Refactor Continuously
- Leave code cleaner than you found it
- Do not batch refactors
- Remove duplication immediately

### 6. Avoid Over-Engineering
- Do the simplest thing that works
- Delay abstractions until needed
- Prefer concrete over speculative design

### 7. No Large Commits
- Commit small, logical changes
- Keep diffs reviewable
- Avoid mixing refactor + feature

### 8. Single Level of Abstraction
- Do not mix high-level and low-level logic
- Each function should operate at one level

### 9. Explicit Over Implicit
- Avoid hidden behavior
- Make dependencies visible
- Prefer clear inputs/outputs

### 10. Professional Discipline
- Do not commit broken code
- Do not skip tests "just this once"
- Be honest about progress and blockers

---

## Code Examples

### ❌ Bad
```ts
function process(data) {
  if (data) {
    // do stuff
  }
}
```

## Commands

```bash
# Find long functions (more than 20 lines)
grep -r "function\|def\|class" src/ | xargs -I {} sh -c 'echo -n "{}: "; wc -l < "{}"' | awk '$2 > 20'

# Check for empty catch blocks
grep -r "catch.*{" src/ | grep -A 5 -B 5 "catch" | grep -v "//"

# Find functions without tests
find src/ -name "*.ts" -o -name "*.js" | xargs grep -l "function\|def" | xargs -I {} sh -c 'test_file="${}"; test_file="${test_file%.ts}.test.ts"; test_file="${test_file%.js}.test.js"; [ ! -f "$test_file" ] && echo "Missing test for: {}"'
```

## Resources

- **Templates**: See [assets/](assets/) for code templates
- **Documentation**: See [references/](references/) for additional clean code guides