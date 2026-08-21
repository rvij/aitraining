# Code Review

Treating AI-generated code as a first draft, not a final answer, is the most important habit to develop. Review everything the AI writes — it produces confident output regardless of correctness.

## Reviewing AI Output

Apply the same standard you'd use for a PR from a junior developer:

- Does it actually do what the prompt asked?
- Are edge cases handled or silently ignored?
- Does it fit the existing style and architecture?
- Are there security implications (SQL injection, path traversal, etc.)?
- Are the tests testing the right things?

## What to Look For

| Red flag | Common cause |
|----------|-------------|
| Bare `except:` | Swallows errors silently |
| `eval()` or `exec()` | Potential code injection |
| Hardcoded credentials | AI sometimes echoes examples literally |
| Missing null checks | AI optimises for the happy path |
| Hallucinated library APIs | AI invents plausible method names |

## Asking AI to Review

You can also ask the AI to review its own output — or code you wrote:

```
Review this code for security issues, error handling gaps,
and anything that would fail under high concurrency.
Be specific about what's wrong and why.
```

!!! tip
    Ask a second AI session to review code produced by the first — it's less likely to repeat the same blind spots.
