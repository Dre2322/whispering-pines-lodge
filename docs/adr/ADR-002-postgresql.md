# ADR-002: Use PostgreSQL as the Primary Database

**Status:** Accepted

## Context
Reservations, rooms, users, and pricing have clear relationships and require data integrity.

## Decision
Use PostgreSQL.

## Rationale
PostgreSQL is a production-grade relational database, integrates well with Django, and provides strong transactional and querying capabilities.
