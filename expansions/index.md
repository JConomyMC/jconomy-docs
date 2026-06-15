---
layout: page
title: Expansions
nav_order: 6
permalink: /expansions/
---

# Expansions

JConomy supports expansions: additional JARs that extend its capabilities at runtime without modifying or forking JConomy itself.

---

## How expansions work

On startup, JConomy scans the `plugins/JConomy/modules/` directory for JAR files and loads any expansions it finds. This directory is created automatically the first time JConomy starts.

Expansions can provide:

- Alternative storage backends (for example, a MySQL or PostgreSQL backend in place of the built-in SQLite backend)
- Import and export providers for the [Data Transfer](../experimental/data-transfer/) feature

---

## Available expansions

No official expansions are currently available.

A MySQL storage expansion is planned but has not been released. Check the [JConomy GitHub project](https://github.com/JConomyMC/jconomy) for updates.

---

## Installing an expansion

When an expansion becomes available, installation is straightforward: place the expansion JAR in `plugins/JConomy/modules/` and restart the server. JConomy will load it automatically on the next startup.
