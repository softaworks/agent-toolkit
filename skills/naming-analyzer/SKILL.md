---
name: naming-analyzer
description: Analyzes code to recommend descriptive, convention-compliant names for variables, functions, classes, and identifiers. Use when the user asks to rename variables, improve identifier names, review naming conventions, find a better name for something, or fix inconsistent naming patterns across a codebase.
---

# Naming Analyzer

Analyze code context to suggest clearer, more descriptive, convention-compliant names for variables, functions, classes, constants, and other identifiers.

## Workflow

1. **Scan identifiers** in the target file or directory:
   - Variables, constants, functions, methods
   - Classes, interfaces, types
   - Files, directories, database columns, API endpoints

2. **Detect naming issues** by checking each identifier against these categories:
   - **Misleading**: name does not match actual behavior (e.g., `getUser` that mutates state)
   - **Vague**: generic names like `data`, `info`, `temp`, `process`
   - **Abbreviated**: unclear shorthand like `usrCfg`, `calcTtl`
   - **Convention violation**: wrong casing for the language or missing boolean prefix
   - **Inconsistent**: mixed conventions within the same project

3. **Resolve language conventions** for the target codebase:
   - **JS/TS**: `camelCase` vars/functions, `PascalCase` classes, `UPPER_SNAKE_CASE` constants, `is/has/can/should` boolean prefixes
   - **Python**: `snake_case` vars/functions, `PascalCase` classes, `UPPER_SNAKE_CASE` constants, `is_/has_/can_` boolean prefixes
   - **Java**: `camelCase` vars/methods, `PascalCase` classes, `UPPER_SNAKE_CASE` constants, `lowercase` packages
   - **Go**: `PascalCase` exported, `camelCase` unexported, all-caps acronyms (`HTTPServer`)

4. **Generate rename suggestions** with reasoning for each:
   - Provide the current name, suggested name, and why
   - Classify severity: Critical (misleading) > Major (vague/unclear) > Minor (convention)

5. **Validate renames** before applying:
   - Check for naming conflicts with existing identifiers in scope
   - Verify all references (imports, call sites) would be updated
   - Confirm renames do not break tests — run the project test suite after applying changes
   - Use IDE-safe rename refactoring when available

## Key Naming Principles

- **Clarity over brevity**: `userConfig` not `usrCfg`, `calculateTotal` not `calcTtl`
- **Booleans read as questions**: `isActive`, `hasPermission`, `canEdit`, `shouldRender`
- **Functions use verb phrases**: `sendEmail()`, `parseJSON()`, `formatCurrency()`
- **Classes use nouns**: `PaymentProcessor`, `EmailValidator` — avoid generic `Manager`/`Helper`
- **Constants include units**: `CACHE_DURATION_MS`, `MAX_FILE_SIZE_MB`
- **Well-known abbreviations are fine**: `html`, `api`, `url`, `id`, `http`
- **Loop counters can be short**: `i`, `j`, `k` in small scopes

## Examples

### Vague vs. specific names
```javascript
// Before
function process(data) { }
const info = getData();

// After
function processPayment(transaction) { }
const userProfile = getUserProfile();
```

### Misleading names (side effects hidden by getter name)
```javascript
// Before — name implies read-only
function getUser(id) {
  const user = fetchUser(id);
  user.lastLogin = Date.now();
  saveUser(user);
  return user;
}

// After — name reflects the mutation
function fetchAndUpdateUserLogin(id) {
  const user = fetchUser(id);
  user.lastLogin = Date.now();
  saveUser(user);
  return user;
}
```

### Boolean naming
```javascript
// Before
const login = user.authenticated;
const status = checkUser();

// After
const isLoggedIn = user.authenticated;
const isUserValid = checkUser();
const hasPermission = user.roles.includes('admin');
```

## Report Format

Present findings as a structured report:

```
## Naming Analysis — [target]
| # | Location | Current | Suggested | Severity | Reason |
|---|----------|---------|-----------|----------|--------|
| 1 | file:line | `getUser` | `fetchAndUpdateUserLogin` | Critical | Hides side effects |
| 2 | file:line | `d` | `currentDate` | Major | Single-letter in large scope |
| 3 | file:line | `API_url` | `API_URL` | Minor | Inconsistent casing |

**Summary**: X items analyzed, Y issues (Z critical, W major, V minor)
```

## Decision Tree

When choosing a name, follow this sequence:
1. **Boolean?** Use `is/has/can/should` prefix
2. **Function?** Use a verb phrase describing the action
3. **Class?** Use a noun in `PascalCase`
4. **Constant?** Use `UPPER_SNAKE_CASE` with descriptive purpose
5. **Variable?** Use a descriptive noun phrase in the language's convention
