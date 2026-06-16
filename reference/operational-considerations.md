---
layout: page
title: Operational Considerations
parent: Reference
nav_order: 1
permalink: /reference/operational-considerations/
---

# Operational Considerations

This page covers operational practices that are important to running JConomy safely and avoiding data loss.

---

## Write-behind cache

JConomy holds recent account data in memory. When a plugin reads or modifies a player's balance, that operation happens against the in-memory copy. Changes are not written to the storage backend immediately.

JConomy flushes the cache on clean shutdown. It also runs a periodic background flush approximately once per minute by default, which bounds the window of data loss if the server is killed unexpectedly. Both behaviors can be tuned — see [Cache configuration](../configuration/#cache) for details.

The practical consequence is:

- Balance changes made during normal server operation are not written to the storage backend immediately.
- The periodic flush (enabled by default) persists dirty records roughly once per minute. Any balance changes since the last successful flush may be lost in a crash.
- On clean shutdown, all remaining dirty records are flushed before the plugin stops.

### Storage extensions

If you are using a non-default storage extension, the extension's backend may have its own write behavior separate from JConomy's cache. Check the extension's documentation to understand whether it introduces additional buffering or has its own flush configuration.

---

## Shutting down cleanly

Always shut your server down using the `stop` command from the server console or a properly configured management wrapper that sends `stop` before terminating the process.

Do not:

- Kill the server process with a task manager, `SIGKILL`, or equivalent.
- Use hosting control panel "force stop" buttons unless you are certain they send `stop` first.
- Let the process be killed by a watchdog or timeout that does not issue a graceful shutdown.

A clean shutdown gives JConomy the opportunity to flush all pending balance writes to the database before the process exits.

---

## Crash recovery

If your server crashes while JConomy is running, the storage backend will be in the state it was in at the last successful flush. The data that is lost is any balance change that occurred after the last flush and was not yet persisted.

After a crash, restart the server normally. JConomy will resume from the last flushed state.

---

## Editing configuration

JConomy reads its configuration file once on startup. It does not support hot-reloading.

To apply configuration changes:

1. Stop the server.
2. Edit `plugins/JConomy/config.yml`.
3. Restart the server.

Changes made to `config.yml` while the server is running will not take effect until the next restart.

---

## The `config-version` key

The `config-version` key at the top of `config.yml` is managed automatically by JConomy. It tracks which configuration migrations have been applied and is incremented when needed.

Do not change this value manually. Setting it to an incorrect value may cause migrations to be skipped or applied more than once.

---

## The database file

The SQLite database file is located at `plugins/JConomy/jconomy.db`. Do not delete, move, or rename this file while the server is running.

If you need to back up the database, do so while the server is stopped. A stopped server guarantees the database is in a fully flushed, consistent state.
