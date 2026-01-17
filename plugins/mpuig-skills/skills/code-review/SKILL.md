---
name: code-review
description: Perform code reviews following Python best practices. Use when reviewing pull requests, examining code changes, or providing feedback on code quality. Covers security, performance, testing, and design review.
---

# Python Code Review

Follow these guidelines when reviewing Python code.

## Review Checklist

### Identifying Problems

Look for these issues in code changes:

- **Runtime errors**: Potential exceptions, None/null issues, index out of bounds
- **Performance**: Unbounded O(n²) operations, N+1 queries, unnecessary allocations
- **Side effects**: Unintended behavioral changes affecting other components
- **Backwards compatibility**: Breaking API changes without migration path
- **ORM queries**: Complex Django/SQLAlchemy ORM with unexpected query performance
- **Security vulnerabilities**: Injection, XSS, access control gaps, secrets exposure

### Design Assessment

- Do component interactions make logical sense?
- Does the change align with existing project architecture?
- Are there conflicts with current requirements or goals?

### Test Coverage

Every PR should have appropriate test coverage:

- Unit tests for business logic
- Integration tests for component interactions
- End-to-end tests for critical user paths

Verify tests cover actual requirements and edge cases. Avoid excessive branching or looping in test code.

### Long-Term Impact

Flag for senior engineer review when changes involve:

- Database schema modifications
- API contract changes
- New framework or library adoption
- Performance-critical code paths
- Security-sensitive functionality

## Feedback Guidelines

### Tone

- Be polite and empathetic
- Provide actionable suggestions, not vague criticism
- Phrase as questions when uncertain: "Have you considered...?"

### Approval

- Approve when only minor issues remain
- Don't block PRs for stylistic preferences
- Remember: the goal is risk reduction, not perfect code

## Common Patterns to Flag

### N+1 Queries (Django)

```python
# Bad: N+1 query
for user in users:
    print(user.profile.name)  # Separate query per user

# Good: Prefetch related
users = User.objects.prefetch_related('profile')
```

### N+1 Queries (SQLAlchemy)

```python
# Bad: N+1 query
for user in session.query(User).all():
    print(user.profile.name)  # Lazy load per user

# Good: Eager load
users = session.query(User).options(joinedload(User.profile)).all()
```

### SQL Injection

```python
# Bad: SQL injection risk
cursor.execute(f"SELECT * FROM users WHERE id = {user_id}")

# Good: Parameterized query
cursor.execute("SELECT * FROM users WHERE id = %s", [user_id])
```

### Mutable Default Arguments

```python
# Bad: Mutable default argument
def add_item(item, items=[]):
    items.append(item)
    return items

# Good: Use None as default
def add_item(item, items=None):
    if items is None:
        items = []
    items.append(item)
    return items
```

### Bare Except Clauses

```python
# Bad: Catches everything including KeyboardInterrupt
try:
    risky_operation()
except:
    pass

# Good: Catch specific exceptions
try:
    risky_operation()
except (ValueError, IOError) as e:
    logger.error(f"Operation failed: {e}")
```

### Resource Management

```python
# Bad: File handle may not be closed
f = open('file.txt')
data = f.read()
f.close()

# Good: Use context manager
with open('file.txt') as f:
    data = f.read()
```

### Type Hints

```python
# Bad: No type information
def process_data(data):
    return data.get('name')

# Good: Type hints for clarity
def process_data(data: dict[str, Any]) -> str | None:
    return data.get('name')
```

### Async/Await Patterns

```python
# Bad: Blocking call in async function
async def fetch_data():
    response = requests.get(url)  # Blocks the event loop
    return response.json()

# Good: Use async HTTP client
async def fetch_data():
    async with httpx.AsyncClient() as client:
        response = await client.get(url)
        return response.json()
```

## References

- [PEP 8 - Style Guide for Python Code](https://peps.python.org/pep-0008/)
- [PEP 484 - Type Hints](https://peps.python.org/pep-0484/)
- [Python Security Best Practices](https://docs.python.org/3/library/security_warnings.html)
