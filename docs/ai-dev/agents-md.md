# AGENTS.md and Instructions

Leaving the AI to make all decisions itself is rarely the right approach. If you care about quality, style, performance, or maintainability, you need to provide guidance. Instruction files let you do this systematically.

## Checklist

- [ ] I know what AGENTS.md is and where to place it
- [ ] I have manually included an instructions file in at least one prompt
- [ ] I have created a custom instructions file for a specific file type

## What is AGENTS.md

Most AI tools recognize an `AGENTS.md` file in the repository root as a source of project-level instructions. You can also place `AGENTS.md` files in subdirectories to apply specific instructions to different parts of your project. The agent uses the nearest one in the directory tree.

This file should contain information relevant to any AI use in the repository, including its purpose, structure, how to build, run, lint, and test. Keep it short — it appears in every context window.

```markdown
# Project: AITraining API

## Purpose
FastAPI application for the AITraining curriculum platform.

## Stack
Python 3.13 | FastAPI | SQLAlchemy | Pytest

## Commands
make test       # run test suite (coverage ≥ 80%)
make lint       # ruff + mypy
make start      # uvicorn dev server on :8000

## Conventions
- Use snake_case for functions, PascalCase for classes
- All endpoints return Pydantic response models
- Never use bare except — catch specific exceptions
- Tests live alongside the code they test (co-location)
```

## Custom Instructions Files

Beyond AGENTS.md, you can create file-type-specific instruction files:

- `.cursorrules` — Cursor-specific rules file at repo root
- `.github/copilot-instructions.md` — GitHub Copilot workspace instructions
- `.claude/CLAUDE.md` — Claude Code project instructions

## Potential Uses

- Enforcing coding standards (naming, error handling, logging)
- Documenting architecture decisions the AI should respect
- Listing what *not* to do (e.g. "never add print statements in production code")
- Specifying test patterns and fixture conventions
- Describing the domain so the AI uses correct terminology

## Instructions from Other Locations

You can manually include any markdown file in a prompt by referencing it in the chat:

```
@docs/architecture.md
Implement the caching layer described in the architecture doc above.
```

This is useful for one-off contexts that don't warrant a permanent instructions file.
