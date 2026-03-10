---
title: Quick Start
description: Get Wezel running in your project.
---

This guide gets you from zero to your first build observations in about ten minutes.

## 1. Deploy the server

Wezel is self-hosted. You'll need the server running before you can connect a project to it.

The quickest path is Docker Compose. See the [Docker Compose guide](/docs/self-hosting/docker-compose) for step-by-step instructions.

Once it's up, note the URL — you'll need it next. For a local deployment that's usually `http://localhost:3001`.

## 2. Initialize your project

In your project root, run:

```sh
wezel setup
```

This creates `.wezel/config.toml` and prompts for your server URL. You can also pass it directly:

```sh
wezel setup --server-url http://your-server:3001
```

Commit `.wezel/config.toml` so your whole team shares the same configuration.

## 3. Start observing builds

Wezel works by wrapping your build tool. For Cargo projects:

```sh
wezel alias cargo
```

This installs a shell alias so every `cargo build`, `cargo test`, `cargo check`, and so on is automatically observed. Restart your shell (or `source ~/.zshrc` / `source ~/.bashrc`) for the alias to take effect.

Run a build to confirm it's working:

```sh
cargo build
```

Then check that everything is connected:

```sh
wezel health
```

## 4. Open the dashboard

Navigate to your server URL in a browser. Your first build observations should already be visible.

From here you can see which build scenarios your team runs most, how long they take across commits, and where time is being spent.

## Next steps

Once you have observations flowing, you can promote the most important ones to **tracked** benchmarks that run on every commit in CI. See the [Forager](/docs/forager/benchmarks) section to get started.
