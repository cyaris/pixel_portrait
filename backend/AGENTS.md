# Backend Guidance

## Shared Conventions

- Inherit the shared Python and SQL conventions in `../../shared-automation/agents/python-sql.md`, which `CLAUDE.md`
  imports.

## Backend Python

- Use `src/utils.py` for backend logging. Backend scripts should initialize loggers with `initialize_logger`.
