---
title: Architecture
description: Internal component breakdown for contributors and integrators.
---

This page covers the internal architecture of Wezel. It's aimed at contributors, integrators, and anyone self-hosting who wants to understand what's running.

## Components

| Name | Role |
|------|------|
| **Burrow** | The backend API server. Built with Axum + Postgres. Ingests build events from Pheromone and Forager, serves historical data to Anthill. Runs on port 3001. |
| **Anthill** | The web dashboard frontend. Visualizes observations, trends, and regression events. |
| **Pheromone** | The local build agent. Ships as `pheromone-<tool>` binaries (e.g. `pheromone-cargo`) that hook into build tools to capture invocations and report them to Burrow. |
| **Forager** | The benchmark runner CLI (`forager`). Runs defined benchmarks against a checked-out commit and reports measurements to Burrow. Ships forager plugins as `forager-<name>` binaries. |

## Repository layout

```
crates/
  wezel_types/      # Shared API and config types (serde, camelCase JSON)
  burrow/           # API server (axum, sqlx, postgres)
  forager_cli/      # `forager` binary entry point
  forager_exec/     # Built-in exec plugin
  forager_llvm_lines/ # Built-in llvm-lines plugin
  pheromone_cargo/  # Cargo build hook
  wezel_cli/        # `wezel` CLI (config, project management)
```

## Burrow API

All HTTP routes are defined in `crates/burrow/src/main.rs`. Key endpoints:

| Method | Path | Description |
|--------|------|-------------|
| `POST` | `/api/project` | Create a project |
| `POST` | `/api/project/:id/observation` | Ingest a build observation from Pheromone |
| `POST` | `/api/forager/claim` | Claim a forager run token |
| `POST` | `/api/forager/run` | Submit benchmark measurements |

## Database

Postgres, migrated via `sqlx::raw_sql` in `crates/burrow/src/db.rs`. Key tables: `projects`, `observations`, `measurements`, `forager_tokens`.

## Forager plugin protocol

Plugins are binaries named `forager-<name>`. The runner communicates with them via environment variables:

| Variable | Direction | Description |
|----------|-----------|-------------|
| `FORAGER_INPUTS` | runner → plugin | JSON-encoded step inputs |
| `FORAGER_OUT` | plugin → runner | Path to write JSON result |
| `FORAGER_STEP` | runner → plugin | Current step name |

Run `forager-<name> --schema` to inspect a plugin's input/output schema.
