---
layout: page
title: Administration
nav_order: 5
permalink: /administration/
---

# Administration

JConomy provides a set of in-game commands for server administrators to manage player accounts and balances. All commands are subcommands of `/jconomy`.

---

## Commands

| Command                                                | Description                                                   |
|--------------------------------------------------------|---------------------------------------------------------------|
| `/jconomy balance get <player> <currency>`             | Display a player's balance for a given currency               |
| `/jconomy balance set <player> <currency> <amount>`    | Set a player's balance for a given currency                   |
| `/jconomy balance add <player> <currency> <amount>`    | Add an amount to a player's balance for a given currency      |
| `/jconomy balance remove <player> <currency> <amount>` | Remove an amount from a player's balance for a given currency |
| `/jconomy account create <player>`                     | Create an economy account for a player                        |
| `/jconomy account delete <player>`                     | Delete a player's economy account and all their balances      |

---

## Balance commands

All balance commands require the player name and a currency name. The currency name must match one of the currencies defined in `config.yml`. See [Currencies](../configuration/currencies/) for how currencies are configured.

Balances are scoped to the world configured as `default-world` in `config.yml`. See [Configuration](../configuration/) for details.

`balance add` and `balance remove` adjust the existing balance. They do not check for negative results — `balance remove` can produce a negative balance.

---

## Account commands

`account create` creates a new economy account for a player. If the player already has an account, no change is made and a message is returned.

`account delete` removes the player's account and **all of their balances across all currencies**. This is permanent and cannot be undone. Use it with care.

---

## Permissions

All administration commands require explicit permission assignment. None are granted by default.

| Permission | Command | Description |
|---|---|---|
| `jconomy.admin.balance.get` | `/jconomy balance get` | View a player's balance |
| `jconomy.admin.balance.set` | `/jconomy balance set` | Set a player's balance |
| `jconomy.admin.balance.add` | `/jconomy balance add` | Add to a player's balance |
| `jconomy.admin.balance.remove` | `/jconomy balance remove` | Remove from a player's balance |
| `jconomy.admin.account.create` | `/jconomy account create` | Create a player's account |
| `jconomy.admin.account.delete` | `/jconomy account delete` | Delete a player's account |

Grant these only to trusted administrators and operators.
