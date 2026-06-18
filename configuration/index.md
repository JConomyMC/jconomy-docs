---
layout: page
title: Configuration
nav_order: 4
permalink: /configuration/
has_children: true
---

# Configuration

JConomy is configured through a single file: `plugins/JConomy/config.yml`.

This file is generated automatically on first startup with default values. You do not need to create it manually. Edit it while the server is stopped, then restart to apply your changes.

---

## File location

```
plugins/
  JConomy/
    config.yml
```

---

## Configuration areas

| Section                    | Purpose                                                               |
|----------------------------|-----------------------------------------------------------------------|
| `default-world`            | The world used by the legacy Vault adapter when no world is specified |
| `default-currency`         | The currency used by the legacy Vault adapter for balance operations  |
| `default-number-formatter` | Formatting rules that apply to all currencies unless overridden       |
| `currencies`               | Definitions for each currency your server uses                        |
| `vault-legacy-adapter`     | Controls whether JConomy registers as a legacy Vault economy provider |
| `cache`                    | In-memory cache settings: size, warming behavior, and periodic flush  |
| `features`                 | Enables experimental features; not present in the default config file |

The `config-version` key at the top of the file is used internally for automatic migration. Do not change it.

---

## Sections in detail

- [Currencies](currencies/) — how to define currencies, set display names and symbols, and control the format of balance strings
- [Number Formatting](number-formatting/) — how to configure grouping separators, decimal places, and rounding
- [Legacy Vault Adapter](legacy-vault-adapter/) — how to enable JConomy as a legacy Vault economy provider

The `features` section is not present in the default `config.yml`. It must be added manually. See [Experimental Features](../experimental/) for details.

---

## Cache

The cache settings control how JConomy manages the in-memory balance cache and how it preloads (warms) balances when players join or teleport.

### Cache size and eviction

```yaml
cache:
  lru-limit: 10000
```

`cache.lru-limit` controls how many player accounts JConomy holds in memory at one time. When the limit is reached, the least recently accessed accounts are evicted from the cache. Evicted accounts are read from disk again on next access.

The default of 10,000 is suitable for most servers. On very large servers you may increase this to reduce disk reads. On resource-constrained servers you may lower it.

This setting does not affect data integrity. Evicting an account from the cache does not delete or flush it.

### Cache warming on join and teleport

```yaml
cache:
  warm-on-join: true
  warm-on-teleport: true
```

Cache warming preloads player balances into memory when they join the server or teleport between worlds. This can improve performance for balance lookups but increases startup I/O.

- `cache.warm-on-join` (default: `true`): When a player joins, JConomy asynchronously loads their balances for the default world and their current location world. This happens in the background without blocking the join process.
- `cache.warm-on-teleport` (default: `true`): When a player teleports to a different world, JConomy asynchronously loads their balance for the destination world.

Both settings are **global toggles**. Individual currencies can override them with per-currency settings — see [Per-currency cache options](../currencies/#per-currency-cache-options).

### Periodic flush

```yaml
cache:
  periodic-flush:
    enabled: true
    interval-ticks: 1200
```

`cache.periodic-flush.enabled` controls whether JConomy runs a background task to periodically write dirty cached records to the storage backend. This is enabled by default. Disabling it turns off only the background task — dirty records are still flushed on clean shutdown.

`cache.periodic-flush.interval-ticks` controls how often the background flush task runs, in ticks. 20 ticks equals one second. The default of 1200 ticks is approximately once per minute at 20 TPS. Reducing this value persists data more frequently at the cost of more frequent disk writes.
