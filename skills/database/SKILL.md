---
name: database
description: Handles database migrations, seeding, schema changes, and test data generation. Use when creating migrations, seeding databases, generating factories/fixtures, or modifying database schemas.
---

# Database

Handles database migrations, schema changes, seed data, factories, and fixtures.

## Migration workflow

1. Detect the migration tool (Prisma, Knex, Alembic, Flyway, Django, Goose, etc.)
2. Examine existing schema and migration history
3. Generate migration file(s) following the tool's naming convention
4. Include both forward (up) and rollback (down) operations
5. Add safety considerations: transactions, concurrent indexes, avoid destructive ops without confirmation

### Confirmation gates

**Before applying destructive operations, ALWAYS ask the user to confirm:**
- Dropping columns or tables
- Changing column types (may lose data)
- Adding NOT NULL columns to tables with existing data
- Removing indexes
- Any operation marked as `risk: high`

### Data backfill

When a migration requires data backfilling:
1. Add column as nullable or with default first
2. Provide a backfill script
3. Generate a follow-up migration to add the constraint

## Seeding workflow

1. Read the database schema (from migrations, Prisma schema, models, etc.)
2. Identify tables, columns, types, constraints, and relationships
3. Generate seed data that respects FK ordering, column types, unique constraints, enums
4. Create either seed scripts (run once), factories (on-demand), or fixtures (static files)

### Seed types

- **Seed scripts**: One-time scripts that populate the database
- **Factories**: On-demand data generators for tests
- **Fixtures**: Static JSON/CSV files for consistent test state

### Detection

Detect existing patterns:
- **Knex**: `seeds/` directory, `knex seed:run`
- **Prisma**: `prisma/seed.ts`, `prisma db seed`
- **Rails**: `db/seeds.rb`, `db/seeds/`
- **Django**: `fixtures/`, `manage.py loaddata`
- **Laravel**: `database/seeders/`, `database/factories/`

## Rules

- Read the actual schema before generating data or migrations
- Respect foreign key ordering (parents before children)
- Use realistic fake data, not "test1", "test2"
- Support the project's existing migration and seed patterns
- Rollback migrations always provided
- If factories exist, extend them rather than creating new ones
- Generate enough data to be useful but not so much it's slow

## Output format

```markdown
## Database Changes

### Migration
- **Tool:** prisma/alembic/knex
- **Risk level:** low/medium/high
- **Reversible:** yes/no

### Changes
| Operation | Table | Column | Details |
|-----------|-------|--------|---------|
| CREATE | users | - | id, email, created_at |
| ADD | posts | author_id | FK to users.id |

### Seed Data
- **File:** `seeds/001_users.js`
- **Records:** 10 users, 5 admins
- **Order:** users → posts → comments (respects FKs)

### Files created
- `migrations/20240101_create_users.sql`
- `migrations/20240101_create_users_rollback.sql`
- `seeds/001_users.js`
```
