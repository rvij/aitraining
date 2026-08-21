# IDE Features

Modern AI extensions expose several modes. Knowing when to use each one is the key to staying in flow rather than fighting the tool.

## Autocomplete

Autocomplete (also called "ghost text") completes code as you type. It works best when:

- You've written a descriptive comment above the code
- You've started the function signature — name and parameters give strong signal
- The surrounding file has similar patterns for it to follow

```python
# Parse an ISO 8601 date string and return a UTC datetime object.
# Raise ValueError if the string is not a valid date.
def parse_date(value: str) -> datetime:
```

The comment and signature together give the assistant everything it needs to produce a correct body.

## Chat Pane

The chat pane is for larger tasks that need back-and-forth: explaining code, generating a new module, or debugging an error. You can **attach files** to give it context beyond what's in the current editor tab.

!!! tip
    Paste error messages verbatim — the full stack trace is far more useful than a paraphrase.

## Plan & Agent Mode

Agent (or "agentic") mode lets the assistant read files, run commands, and make edits autonomously. Use it for multi-step tasks:

- Implementing a feature across several files
- Running tests and fixing failures in a loop
- Refactoring a module and updating all its callers

!!! warning
    Agent mode can make many edits fast. Review diffs carefully before accepting — it doesn't always know when to stop.

## Inline Edits

Select a block of code and invoke the inline edit command (`⌘K` in VS Code). Describe the change in plain English:

```
"Add input validation — raise ValueError if name is empty or longer than 100 characters"
```

Inline edits are precise and reversible. Prefer them over chat for small, targeted changes.
