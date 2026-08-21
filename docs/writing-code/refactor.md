# Refactoring Code

AI assistants handle mechanical refactors — renaming, extracting, reorganizing — very well. For structural refactors, guide it with the target shape.

## When to Refactor

- A function is longer than ~40 lines and handles multiple concerns
- The same logic appears in 3+ places
- A parameter list has grown beyond 4–5 items
- Tests are hard to write because the unit is too large

## Safe Refactoring Steps

1. Make sure tests pass *before* starting
2. Ask for one refactor at a time
3. Run tests after each change
4. Review the diff before accepting

## Example Prompts

**Extract a helper function:**

```
Extract the date parsing logic (lines 12–28) into a
separate function called parse_event_date.
Keep the same behaviour and update the caller.
```

**Replace magic numbers:**

```
Replace all magic number literals in this file with
named constants at the top of the module.
Use SCREAMING_SNAKE_CASE.
```

**Convert to dataclass:**

```
Convert this dictionary-based config object to a
Python dataclass. Preserve all existing fields.
Add type annotations based on how they're used.
```
