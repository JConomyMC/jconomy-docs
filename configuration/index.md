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
| `default-world`            | The world used for balance lookups when no world is specified         |
| `default-currency`         | The currency exposed by the legacy Vault adapter                      |
| `default-number-formatter` | Formatting rules that apply to all currencies unless overridden       |
| `currencies`               | Definitions for each currency your server uses                        |
| `vault-legacy-adapter`     | Controls whether JConomy registers as a legacy Vault economy provider |
| `cache`                    | In-memory account cache size                                          |

The `config-version` key at the top of the file is used internally for automatic migration. Do not change it.

---

## Sections in detail

- [Currencies](currencies/) — how to define currencies, set display names and symbols, and control the format of balance strings
- [Number Formatting](number-formatting/) — how to configure grouping separators, decimal places, and rounding
- [Legacy Vault Adapter](legacy-vault-adapter/) — how to enable JConomy as a legacy Vault economy provider

---

## Cache

```yaml
cache:
  lru-limit: 10000
```

`cache.lru-limit` controls how many player accounts JConomy holds in memory at one time. When the limit is reached, the least recently accessed accounts are evicted from the cache. Evicted accounts are read from disk again on next access.

The default of 10,000 is suitable for most servers. On very large servers you may increase this to reduce disk reads. On resource-constrained servers you may lower it.

This setting does not affect data integrity. Evicting an account from the cache does not delete or flush it.
