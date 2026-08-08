# ADR-001: Use Django as the Web Framework

**Status:** Accepted

## Context
The application requires authentication, forms, database access, migrations, administrative tooling, and server-rendered pages.

## Decision
Use Django for Version 1.

## Rationale
Django provides mature implementations for these needs and allows the project to focus on reservation/business logic rather than assembling framework components.
