---
layout: page
title: Before You Begin
nav_order: 2
permalink: /before-you-begin/
---

# Before You Begin

Read this page before installing JConomy. It sets expectations about what JConomy does and does not do, so you can determine whether it fits your server before you configure anything.

---

## JConomy is infrastructure, not an application

JConomy manages economy data. It handles how balances are stored, formatted, and exposed to other plugins — not how they are earned, spent, or administered.

What JConomy provides:

- A persistent store for player balances, per world and per currency
- Currency definitions and formatting (symbols, decimal precision, display names)
- Registration with VaultUnlocked as the active economy provider on your server
- An optional adapter for plugins that use the older legacy Vault API

What it does not provide:

- **No player commands.** JConomy registers no commands that players or administrators use for economy actions. There is no `/balance`, `/pay`, `/eco`, `/baltop`, or equivalent.
- **No shop or GUI interfaces.**
- **No bank or shared accounts.** JConomy tracks balances for individual player accounts only.
- **No gameplay economy logic.** How balances change — through jobs, shops, events, or administrative actions — is the responsibility of whichever plugin governs that gameplay.

---

## You still need economy command plugins

Because JConomy provides no player-facing commands, most servers will need a separate plugin to fill that role. The two most common approaches are:

**Use EssentialsX alongside JConomy.** EssentialsX provides `/balance`, `/pay`, `/eco`, and related commands. When EssentialsX detects an active Vault economy provider, it delegates balance operations to that provider. With JConomy installed and active, EssentialsX will use JConomy for persistence while still handling all command and player interaction concerns itself.

**Use another Vault-compatible economy commands plugin.** Any plugin that uses VaultUnlocked or legacy Vault to read and write balances will work with JConomy as the backend.

There is no migration required for existing economy command plugins. They continue to work exactly as they do today once JConomy is registered as the provider.

---

## VaultUnlocked is required

JConomy requires VaultUnlocked to be installed. VaultUnlocked is the service registry that connects JConomy to the rest of your plugin ecosystem. Without it, JConomy will disable itself on startup.

The legacy Vault plugin is also required to be present (even if you do not plan to use the legacy adapter), due to how JConomy declares its plugin dependencies. See [Installation](../installation/) for details.

---

## SQLite is the default storage backend

JConomy ships with a built-in SQLite storage backend. On first startup it creates a database file at `plugins/JConomy/jconomy.db`. No additional database software is required.

Alternative storage backends may be added through the expansion mechanism. No alternative storage expansions are officially available yet.

---

## Balance writes are held in memory during server runtime

JConomy uses a write-behind cache. Balance changes made during normal server operation are held in memory and written to disk only when the server shuts down cleanly. If your server crashes or is forcibly killed, any balance changes that occurred since the last clean shutdown may be lost.

This is worth knowing before you go live. Ensure your server shuts down cleanly and avoid killing the process forcibly. See [Operational Considerations](../reference/operational-considerations/) for more detail.
