---
layout: page
title: Installation
nav_order: 3
permalink: /installation/
---

# Installation

This page covers installing JConomy on a Spigot or Paper server from scratch.

If you have not already read [Before You Begin](../before-you-begin/), do that first. It describes what JConomy does and does not do, and what other providers must or must not be present on your server before you install.

---

## Requirements

| Requirement | Version |
|---|---|
| Spigot or Paper | 1.21 or later |
| Java | 21 or later |
| VaultUnlocked | Required |

**VaultUnlocked replaces Vault.** If you currently have Vault installed, remove it and install VaultUnlocked instead. VaultUnlocked is a drop-in replacement: it provides all of the original Vault API at the same class locations, so every plugin that worked with Vault will continue to work with VaultUnlocked installed. You do not need both.

---

## Download

Download the latest `JConomy.jar` from the [JConomy GitHub project page](https://github.com/JConomyMC/jconomy).

---

## Steps

1. **Install [VaultUnlocked](https://github.com/TheNewEconomy/VaultUnlocked)** into your `plugins/` folder. If you currently have Vault installed, remove it first — VaultUnlocked replaces it entirely.

2. **Remove any conflicting VaultUnlocked economy provider.** Only one VaultUnlocked economy provider can be active at a time. If you have another plugin registered as the VaultUnlocked provider, remove it before continuing. See [Before You Begin](../before-you-begin/#compatibility-with-other-economy-providers) for details.

3. **Drop `JConomy.jar`** into your server's `plugins/` folder.

4. **Start the server.** JConomy will generate its configuration files and database on first startup.

5. **Confirm JConomy loaded.** Bukkit logs a message for each plugin it loads on startup. Check the server console output for JConomy's name. If it does not appear, or if an error is logged alongside it, check that VaultUnlocked loaded successfully first — JConomy will disable itself and log an error if VaultUnlocked is missing.

6. **Stop the server** and configure JConomy before going live. At minimum, define your currencies in `plugins/JConomy/config.yml`.

7. **Restart the server** to apply your configuration.

---

## Files generated on first startup

On first startup, JConomy creates the following files and directories:

```
plugins/
  JConomy/
    config.yml       ← default configuration file
    jconomy.db       ← SQLite database (created automatically)
    extensions/      ← directory for extension JARs (empty by default)
```

You do not need to create any of these manually. If `config.yml` is absent when the server starts, JConomy will generate it with defaults.

---

## Next steps

Once JConomy is running, proceed to [Configuration](../configuration/) to define your currencies and adjust settings for your server.
