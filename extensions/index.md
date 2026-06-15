---
layout: page
title: Extensions
nav_order: 6
permalink: /extensions/
---

# Extensions

JConomy supports extensions: additional JARs that extend its capabilities at runtime without modifying or forking JConomy itself.

---

## How extensions work

On startup, JConomy scans the `plugins/JConomy/extensions/` directory for JAR files and loads any extensions it finds. This directory is created automatically the first time JConomy starts.

Extensions can provide:

- Alternative storage backends (for example, a MySQL or PostgreSQL backend in place of the built-in SQLite backend)
- Import and export providers for the [Data Transfer](../experimental/data-transfer/) feature

---

## Available extensions

No official extensions are currently available.

A MySQL storage extension is planned but has not been released. Check the [JConomy GitHub project](https://github.com/JConomyMC/jconomy) for updates.

---

## Installing an extension

When an extension becomes available, installation is straightforward: place the extension JAR in `plugins/JConomy/extensions/` and restart the server. JConomy will load it automatically on the next startup.
