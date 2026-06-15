---
layout: page
title: Legacy Vault Adapter
parent: Configuration
nav_order: 3
permalink: /configuration/legacy-vault-adapter/
---

# Legacy Vault Adapter

JConomy registers itself as the active economy provider with VaultUnlocked. It does not register as a legacy Vault economy provider by default.

If your server has plugins that use the original Vault API (`net.milkbowl.vault.economy.Economy`) and have not been updated to use VaultUnlocked, you can opt in to registering JConomy as the legacy Vault provider as well.

---

## Enabling the adapter

In `plugins/JConomy/config.yml`, set `vault-legacy-adapter.enabled` to `true`:

```yaml
vault-legacy-adapter:
  enabled: true
```

Restart the server to apply the change.

---

## Prerequisites

Before enabling the adapter, remove any plugin that is currently registered as the legacy Vault economy provider. Only one legacy Vault economy provider can be active at a time. If another provider is already registered when JConomy starts, the two will conflict.

See [Before You Begin](../../before-you-begin/#compatibility-with-other-economy-providers) for a full explanation of provider conflicts.

---

## Currency and world used by the adapter

The legacy Vault API is single-currency and does not include a world parameter on most operations. When JConomy receives a legacy Vault call, it routes it using two configuration keys:

| Key | Purpose |
|---|---|
| `default-currency` | The currency used for all legacy Vault balance operations |
| `default-world` | The world used when no world is specified |

Both keys are set at the top level of `config.yml`, not under `vault-legacy-adapter`. Make sure `default-currency` matches the key of a currency you have defined in the `currencies` section.

These settings have no effect on VaultUnlocked calls, which always specify currency and world explicitly.

---

## Banking is not supported

The legacy Vault API includes bank operations (`bankBalance`, `bankDeposit`, `bankWithdraw`, and related methods). JConomy does not implement shared or bank accounts. All bank-related calls return a `NOT_IMPLEMENTED` response.

If you have plugins that rely on bank functionality, the legacy Vault adapter will not satisfy those requirements.
