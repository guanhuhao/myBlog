---
name: collaborating-with-grok
description: |
  Programming fact-checker for API deprecation, version changes, and community pitfalls.
  Auto-triggered for: (1) Version queries like "React 18 hooks" (2) Error messages mentioning deprecated APIs
  (3) Tech comparisons or migration questions. NOT for code generation—only verification.
---

# Collaborating with Grok

Verify programming facts via real-time web search.

## Triggers

- Function signature queries: "What's the signature of `torch.compile`?"
- Deprecation checks: "Is `componentWillMount` still valid?"
- Version-specific APIs: "How to use Next.js 14 server actions?"

## Workflow

### 1. Intent Classification

```
Query → [Signature Lookup] or [Deprecation Check]
```

### 2. Execution

**Signature Lookup:**
```
mcp__grok-search__web_search(query="{library} {function} signature site:docs")
→ Extract: params, return type, version
```

**Deprecation Check:**
```
mcp__grok-search__web_search(query="{API} deprecated OR removed {year}")
→ Return: status, replacement, migration path
```

### 3. Output Format

```markdown
## {Function/API}
- **Status**: Active | Deprecated | Removed
- **Signature**: `fn(param: Type) -> ReturnType`
- **Since**: v{version}
- **Note**: {breaking changes or alternatives}
- **Source**: [Official Docs](url)
```

## Constraints

- Always cite official docs over blog posts
- Flag version-specific behavior explicitly
- Never generate code—only verify facts
