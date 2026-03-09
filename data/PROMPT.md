# Data Profile Artefact Compiler — Prompt

You are compiling a structured knowledge artefact about the real-world data behind a single entity from a codebase. The artefact will be consumed by LLMs and humans for tasks like writing performant queries, understanding data distributions, identifying anomalies, and making informed decisions about schema changes.

**This artefact works best when you have direct access to a database where the data is highly similar to what is on production (across tenants, clusters, etc.).**

## Input

The user will provide:

- **Entity name** (e.g. `User`, `Order`, `Invoice`)
- **Entity artefact path** (recommended) — path to an existing entity artefact for this entity
- **Codebase path** (fallback) — used to discover schema if no entity artefact is provided

## Instructions

If an entity artefact path is provided, use its Schema, Relationships, and Indexes sections as your starting point. This tells you the table name, columns, types, foreign keys, and indexes — do not rediscover what is already compiled.

If no entity artefact is provided, explore the codebase to identify the entity's table name, columns, and relationships before profiling.

Then query the database to profile the entity's actual data. Adapt your profiling approach to the table size — prefer exact counts for small tables, approximations for large ones. Do NOT write or modify any data — this is read-only exploration.

Produce a single markdown artefact covering the sections below. Not every metric applies to every column — use judgement and omit irrelevant metrics rather than reporting meaningless values. If a section has no relevant findings, include it with "None found." Do not omit sections. Do not speculate — only report what the data shows.

---

## Artefact Template

```markdown
---
entity: { Entity Name }
table: { schema.table_name }
compiled_at: { ISO 8601 timestamp }
environment: { production | staging | development | unknown }
row_count: { total rows }
caveats: { e.g. "sampled from staging, ~10% of production volume" or "none" }
---

# {Entity Name} — Data Profile

## Table Shape

- Total row count
- Approximate growth rate if timestamp columns allow (e.g. rows per day/week/month over a recent period)
- Soft-delete ratio if applicable (e.g. % of rows where `deleted_at` is not null)

## Column Profiles

For each column, report what is relevant to its type. Not every metric applies to every column.

### {column_name}

**All columns:**

- Null rate (% of rows where value is NULL)
- Distinct value count (exact or approximate for high-cardinality columns)

**String / enum columns — also include:**

- Top N most common values with counts and percentages
- Min / max / average length
- Whether values look like an enum (low cardinality relative to row count)

**Numeric columns — also include:**

- Min, max, mean, median
- Standard deviation if useful
- Distribution shape if notable (e.g. heavily skewed, bimodal, clustered)
- Zero rate if relevant

**Boolean columns — also include:**

- True / false / null distribution

**Timestamp / date columns — also include:**

- Earliest and latest values
- Whether the column is monotonically increasing (insertion timestamps)
- Gap analysis if relevant (e.g. no records between date X and date Y)
- Distribution by time bucket (e.g. records per month) if useful

**Foreign key columns — also include:**

- Null rate (optional vs required relationships)
- Referential density: how many distinct referenced IDs exist out of possible
- Concentration: whether a small number of referenced IDs account for most rows (e.g. "top 10 accounts hold 80% of orders")

**JSON / JSONB columns — also include:**

- Top-level key frequency (which keys appear and how often)
- Common structural patterns
- Empty object / empty array / null rates

## Relationship Cardinality

For each relationship defined in the entity artefact or discovered from foreign keys:

- Actual cardinality distribution (e.g. "users have between 0 and 12,000 orders, median 3, p95 47")
- Orphan rate if applicable (rows referencing non-existent parents)
- Percentage of parent rows with zero children

## Index Utilisation

For each index on the table:

- Whether the index is actually used (from `pg_stat_user_indexes` or equivalent)
- Approximate scan count if available
- Unused indexes flagged explicitly

## Access Statistics

If database statistics are available (e.g. `pg_stat_user_tables` or equivalent):

- Sequential scan vs index scan ratio
- Live tuples vs dead tuples (vacuum health)
- HOT update ratio
- Last vacuum / last analyze timestamps

## Triggers

Database-level triggers defined on the table. For each:

- Trigger name
- When it fires (BEFORE / AFTER, INSERT / UPDATE / DELETE)
- What it does (e.g. auto-sets `updated_at`, populates a search vector, enforces a constraint, logs to an audit table)

## Partitioning

If the table is partitioned:

- Partition strategy (range, list, hash)
- Partition key (which column)
- Number of partitions
- Partition boundaries if range-based (e.g. monthly partitions, date ranges)

## Derived Tables

Materialized views, summary tables, or other derived tables built from this entity. For each:

- Name and type (materialized view, summary table, cache table, etc.)
- What it contains (which columns, aggregations, filters)
- How it's refreshed (manually, on a schedule, trigger-based)
- When it was last refreshed if detectable

## Anomalies

Flag anything that stands out:

- Columns that are 100% null (dead columns)
- Columns with a single distinct value (constants)
- Unexpected null rates on columns that constraints suggest should be required
- Foreign keys with high orphan rates
- Indexes that are never used
- Tables with high dead tuple ratios
- Enum-like string columns that might benefit from a proper enum type
- Timestamp columns with suspicious gaps
```

## Rules

1. **Report what is, not what should be** — This is a snapshot of reality. Do not recommend fixes or improvements — that is the consuming LLM's job.
2. **Omit irrelevant metrics** — A boolean column does not need a median. A high-cardinality UUID column does not need top N values. Use judgement.
3. **Scale to table size** — Prefer exact counts for small tables, approximations for large ones. Do not run expensive queries on billion-row tables when estimates suffice.
4. **Always state provenance** — The environment, timestamp, and any caveats are mandatory. A profile without provenance is misleading.
5. **Bootstrap from the entity artefact** — It already knows the schema, relationships, and indexes. Do not rediscover what is already compiled.
6. **Be precise** — Use exact column names, table names, and values from the database. No paraphrasing.
7. **Readable tables** — If using markdown tables, keep them human-readable: align columns, use concise cell values, and break wide tables into multiple narrower tables rather than producing a single table that requires horizontal scrolling.
