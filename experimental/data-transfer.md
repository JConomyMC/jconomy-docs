---
layout: page
title: Data Transfer
parent: Experimental Features
nav_order: 1
permalink: /experimental/data-transfer/
---

# Data Transfer

{: .warning }
This is an experimental feature. It is not stable, may contain defects, and may cause data loss. It is not officially supported. It may change incompatibly or be removed entirely in any future version without notice. Do not use it in production without testing and without backing up your data first.

The `data-transfer` feature enables commands for importing and exporting player economy account data between providers. Its primary use case is migrating balance data from another economy plugin to JConomy when switching backends.

---

## Enabling data transfer

The `data-transfer` feature is disabled by default. To enable it, add the following to `plugins/JConomy/config.yml` and restart the server:

```yaml
features:
  data-transfer:
    enabled: true
```

No commands are registered until this feature is enabled.

---

## How it works

Data transfer uses a two-step workflow: **preview**, then **execute**.

1. **Preview** — Run the preview command for the desired operation. JConomy generates a transfer plan describing what would be imported or exported and stores it in memory. No data is moved at this stage.
2. **Execute** — Run the execute command. JConomy applies the stored plan and performs the actual data operation.

You must run a preview before running execute. The stored plan is held in memory and is lost if the server restarts. If the server restarts between preview and execute, run the preview again before executing.

---

## Conflict policy

When importing or exporting, account records from the source may conflict with records that already exist in the destination. The conflict policy controls how these are handled.

| Policy | Behavior |
|---|---|
| `SKIP` | Conflicting records in the destination are left unchanged. This is the default. |
| `OVERWRITE` | Conflicting records in the destination are replaced with the source data. |

{: .warning }
`OVERWRITE` is destructive. Existing balances in the destination will be permanently replaced. Back up your data before using `OVERWRITE`.

The conflict policy is an optional argument to the preview command. If omitted, `SKIP` is used.

---

## Commands

All commands are under `/jconomy` and are only available when `data-transfer` is enabled.

| Command                                       | Permission               | Description                                      |
|-----------------------------------------------|--------------------------|--------------------------------------------------|
| `/jconomy import list`                        | `jconomy.list.import`    | Lists available import providers                 |
| `/jconomy export list`                        | `jconomy.list.export`    | Lists available export providers                 |
| `/jconomy import <provider> preview [policy]` | `jconomy.preview.import` | Generates and stores an import transfer plan     |
| `/jconomy export <provider> preview [policy]` | `jconomy.preview.export` | Generates and stores an export transfer plan     |
| `/jconomy import <provider> execute`          | `jconomy.execute.import` | Executes the most recently previewed import plan |
| `/jconomy export <provider> execute`          | `jconomy.execute.export` | Executes the most recently previewed export plan |

---

## Providers

Import and export providers are supplied by extensions. Without an extension that registers a compatible provider, the list commands will return no results and there will be nothing to import from or export to.

See [Extensions](../../extensions/) for more information about the extension mechanism.

---

## Permissions

The data transfer commands use the following permission nodes. None of these are granted by default — they must be assigned explicitly.

| Permission | Description |
|---|---|
| `jconomy.list.import` | List available import providers |
| `jconomy.list.export` | List available export providers |
| `jconomy.preview.import` | Preview an import operation |
| `jconomy.preview.export` | Preview an export operation |
| `jconomy.execute.import` | Execute a previously previewed import |
| `jconomy.execute.export` | Execute a previously previewed export |

Grant these only to trusted administrators and operators.
