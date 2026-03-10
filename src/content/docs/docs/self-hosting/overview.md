---
title: Self-Hosting Overview
description: Run the entire Wezel stack on your own infrastructure.
---

Wezel is fully self-hostable. Every component is MIT-licensed and designed to run on infrastructure you control.

## Components

| Component | Role |
|-----------|------|
| **API server** | Backend API and data store. Ingests build events, serves history. |
| **Dashboard** | Web UI. Visualizes observations, trends, and regressions. |
| **Local build agent** | Hooks into your shell and captures build invocations. |
| **Benchmark runner** | CLI tool for running benchmarks against commits in CI. |

## Prerequisites

- Docker and Docker Compose (v2+)
- A Linux host with at least 1 GB RAM
- Outbound network access from your CI runners to the API server

## Deployment options

### Docker Compose (recommended)

The quickest way to get started. See the [Docker Compose guide](/docs/self-hosting/docker-compose).

### Custom

Each component is a standalone binary or container image. You can run them behind your own reverse proxy and connect them to an existing Postgres instance.

For internal component names and architecture details, see [Architecture](/docs/developing/architecture).

## Data

Wezel stores build event data in Postgres. There is no telemetry and no call-home behaviour.
