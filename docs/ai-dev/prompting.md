# Effective Prompting

Prompt quality is the single biggest lever on output quality. A vague prompt produces vague code. A specific prompt with examples produces code you can use.

## The Anatomy of a Good Prompt

A complete prompt has four parts — not all are needed every time, but missing them causes the most common failures:

| Part | Example | Why |
|------|---------|-----|
| **Context** | "In this FastAPI app..." | Frames the environment |
| **Task** | "Add a `/healthz` endpoint..." | The actual ask |
| **Constraints** | "...that returns JSON, no auth required" | Narrows the solution space |
| **Example** | "Similar to the existing `/ready` endpoint" | Anchors style and pattern |

## Context Techniques

- **Paste the error** — include the full stack trace, not a summary
- **Show, don't tell** — "do it like `X`" beats "do it the standard way"
- **State the negative** — "don't use global state", "no external dependencies"
- **Specify the output format** — "return a Python dict", "output a single function, no class"

## Prompt Patterns

**Implement from signature:**

```python
"""
Given this function signature and docstring, implement the body.
Don't change the signature.
"""
def calculate_churn_rate(
    active_users_start: int,
    active_users_end: int,
    new_users: int,
) -> float:
    """Return the churn rate as a percentage (0–100)."""
```

**Fix with context:**

```
This function is failing with:
    ValueError: invalid literal for int() with base 10: ''

Here is the function:
[paste code]

Fix it to handle empty strings by returning None.
```

## Iteration

Treat the first output as a draft. Refine with follow-up prompts:

- "Make it handle the case where `user` is None"
- "Extract the validation logic into a separate function"
- "Add a unit test for the empty-string case"

Short follow-ups on a specific output are faster than starting over with a longer prompt.
