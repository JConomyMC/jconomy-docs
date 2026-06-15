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

**Use another Vault-compatible economy commands plugin.** Any plugin that acts as a _consumer_ of VaultUnlocked or legacy Vault — reading and writing balances through the API — will work with JConomy as the backend.

The distinction that matters: a **consumer** plugin queries the economy provider for balance data (EssentialsX, shop plugins, reward plugins). A **provider** plugin registers itself as the economy backend. JConomy is a provider. Only one VaultUnlocked provider can be active on a server at a time — see below.

There is no migration required for existing economy consumer plugins. They continue to work exactly as they do today once JConomy is registered as the provider.

---

## VaultUnlocked is required

JConomy requires VaultUnlocked to be installed. VaultUnlocked is the service registry that connects JConomy to the rest of your plugin ecosystem. Without it, JConomy will disable itself on startup.

If you currently have Vault installed, replace it with VaultUnlocked. VaultUnlocked is a drop-in replacement for Vault: it provides all original Vault API classes at the same locations, so every plugin that worked with Vault will continue to work unchanged. You do not need both installed.

### Compatibility with other economy providers

**VaultUnlocked providers always conflict.** JConomy always registers as the VaultUnlocked economy provider. Only one VaultUnlocked provider can be active on a server at a time. If another plugin is already registered as the VaultUnlocked provider, the two will conflict. You must remove the other provider before installing JConomy.

**Legacy Vault providers only conflict if you opt in.** By default, JConomy does not register as a legacy Vault provider. If you have an existing legacy Vault economy plugin on your server, it will continue to operate without interference. Enabling JConomy as a legacy Vault provider is an explicit opt-in step — and if you do enable it, you will need to remove your existing legacy Vault economy plugin first, as the same one-provider rule applies.

See [Legacy Vault Adapter](../configuration/legacy-vault-adapter/) for configuration details.

Plugins that only _consume_ the Vault or VaultUnlocked API (shops, command plugins, reward systems) are not providers and are not affected by either rule. They will continue to work once JConomy is registered.

---

## SQLite is the default storage backend

JConomy ships with a built-in SQLite storage backend. On first startup it creates a database file at `plugins/JConomy/jconomy.db`. No additional database software is required.

Alternative storage backends may be added through the expansion mechanism. No alternative storage expansions are officially available yet.

---

## Balance writes are held in memory during server runtime

JConomy uses a write-behind cache. Balance changes made during normal server operation are held in memory and written to the storage backend when a flush occurs — on clean shutdown, or periodically if configured. If your server crashes or is forcibly killed before a flush, any balance changes since the last flush may be lost.

This is worth knowing before you go live. Ensure your server shuts down cleanly and avoid killing the process forcibly. See [Operational Considerations](../reference/operational-considerations/) for more detail.
