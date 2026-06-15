---
layout: home
title: JConomy
nav_order: 1
---

# JConomy

JConomy is a shared economy infrastructure layer for Spigot and Paper servers. It handles persistence, currency definition, account management, and Vault integration so that other plugins on your server don't have to.

JConomy does not replace your economy applications. It provides the shared backend that those applications—and your custom plugins—build on top of.

---

## What JConomy does

JConomy manages the storage and exposure of balances for any number of named currencies. When JConomy is running, it registers itself as the active economy provider with VaultUnlocked. Any plugin that queries VaultUnlocked for balance information will be talking to JConomy.

Specifically, JConomy provides:

- Persistent storage for player balances, scoped by world and currency
- Definition and formatting of currencies, including symbols, display names, and decimal configuration
- Registration with VaultUnlocked as the economy service provider
- An optional legacy Vault adapter for plugins that have not yet adopted VaultUnlocked
- An in-memory account cache to reduce disk I/O during normal operation
- An expansion mechanism for loading additional capabilities at runtime

---

## What JConomy does not do

JConomy is intentionally limited in scope. It does not provide:

- **Economy commands.** There is no `/balance`, `/pay`, `/eco`, `/baltop`, or any equivalent. Those are the responsibility of the plugin that defines the gameplay rules for a given currency.
- **Player-facing interfaces.** No chat menus, GUIs, or shop systems.
- **Bank or shared accounts.** Shared account support is explicitly not implemented.
- **Economy gameplay logic.** JConomy does not define how balances are earned, spent, or transferred. That belongs to the plugins that model those systems.

If you currently use EssentialsX or another economy application for player commands and shop integration, you can continue to do so. JConomy and EssentialsX can coexist on the same server.

---

## Where JConomy fits

A typical setup looks like this:

```
Your plugins and applications
        ↓  (query via VaultUnlocked API)
     VaultUnlocked
        ↓  (routes to registered economy provider)
       JConomy
        ↓  (reads/writes)
    SQLite database
```

JConomy sits between VaultUnlocked and your storage layer. Plugins on your server do not talk to JConomy directly; they call into VaultUnlocked and JConomy answers.

---

## Who this documentation is for

This site is written for **Minecraft server administrators and operators** who are installing and configuring JConomy on a server they manage.

It assumes you are comfortable editing YAML configuration files and managing Bukkit-based plugins.

If you are a plugin developer looking to integrate with JConomy programmatically, refer to the separate API documentation.

---

## Before you continue

New to JConomy? Read [Before You Begin](before-you-begin/) before installing. It sets expectations about what JConomy does and does not provide.

Once installed and configured, see [Experimental Features](experimental/) if you want to enable data transfer commands, and [Reference](reference/) for commands, permissions, and operational guidance.

