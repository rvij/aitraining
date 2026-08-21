# Introduction to AI-Assisted Development

Working with an AI coding assistant is a different skill from writing code alone. The assistant is fast and knowledgeable but operates only on what you give it. Your job shifts from writing every line to specifying intent clearly and reviewing critically.

## What Changes

With an AI assistant, the bottleneck moves from *typing* to *thinking*. You spend more time on:

- Articulating what you want precisely
- Breaking problems into pieces the assistant can handle
- Reviewing generated code for correctness and fit
- Knowing when to take back control and write manually

## The Mental Model

Think of the AI as a very fast, knowledgeable but forgetful colleague. It:

- Has broad knowledge of libraries, patterns, and idioms
- Can only see what's in its context window — not your whole codebase
- Will confidently produce plausible-but-wrong code if your prompt is ambiguous
- Improves significantly when you give it examples, constraints, and existing code to work from

## What AI Does Well

| Task | AI value |
|------|----------|
| Boilerplate code | Very high — saves 80–90% of the time |
| Test generation | High — especially edge cases and fixtures |
| Refactoring known patterns | High — renaming, extracting, reorganizing |
| Architecture decisions | Low — requires deep project context |
| Novel algorithm design | Low — AI recombines; doesn't invent |

## Common Pitfalls

- **Accepting output without reading it.** Treat AI code like a PR — review every line.
- **Too large a scope per prompt.** Ask for one thing at a time.
- **Missing context.** Paste the relevant file, function signature, or error message.
- **No instructions file.** Without an `AGENTS.md`, the AI doesn't know your conventions.
