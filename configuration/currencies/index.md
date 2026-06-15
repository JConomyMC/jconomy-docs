---
layout: page
title: Currencies
parent: Configuration
nav_order: 1
permalink: /configuration/currencies/
---

# Currencies

Currencies are defined in the `currencies` section of `plugins/JConomy/config.yml`. Each entry in this section defines one currency that JConomy will manage.

---

## Currency keys

Each currency is identified by a key — a plain string you choose. This key is used when constructing account lookups and when other parts of the configuration reference the currency by name (for example, `default-currency`).

```yaml
currencies:
  gold:
    # ...
  silver:
    # ...
```

In this example, `gold` and `silver` are the currency keys.

---

## Currency fields

| Field                   | Required    | Description                                                                                                      |
|-------------------------|-------------|------------------------------------------------------------------------------------------------------------------|
| `display-name-singular` | Recommended | Singular display name, used when the formatted amount is exactly 1                                               |
| `display-name-plural`   | Recommended | Plural display name, used in all other cases                                                                     |
| `symbol`                | No          | A short symbol displayed alongside the amount. Supports Minecraft color and formatting codes. Defaults to empty. |
| `format-string`         | No          | Controls how the full currency string is assembled. See [Format strings](#format-strings) below.                 |
| `number-formatter`      | No          | Per-currency override of the global number formatting rules. See [Number Formatting](../number-formatting/).     |

If neither `display-name-singular` nor `display-name-plural` is provided, JConomy will fall back to whichever one is present. Providing both is recommended.

---

## Format strings

The `format-string` field controls how JConomy assembles the final display string for an amount. It supports the following placeholders:

| Placeholder          | Description                                                                                                                                               |
|----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `%amount_formatted%` | The amount run through the number formatter. Applies grouping separators, decimal places, and rounding as configured. Recommended for displaying amounts. |
| `%amount_raw%`       | The exact numeric amount with no formatting applied.                                                                                                      |
| `%symbol%`           | The currency symbol.                                                                                                                                      |
| `%display_name%`     | The singular display name if the rounded amount equals 1; the plural display name otherwise.                                                              |
| `%sign%`             | A minus sign (`-`) if the amount is negative; nothing if it is positive or zero.                                                                          |

The default format string is `%sign%%symbol%%amount_formatted%`, which produces output like `$1,234.56` or `-$1,234.56`.

---

## Example

The following is a complete currency definition:

```yaml
currencies:
  gold:
    display-name-singular: Gold Coin
    display-name-plural: Gold Coins
    symbol: '&6G'
    format-string: '%sign%%amount_formatted% %symbol%'
    number-formatter:
      fractional:
        places: 0
```

This definition:

- Uses `gold` as the currency key.
- Displays `Gold Coin` when the balance is exactly 1, and `Gold Coins` otherwise.
- Uses a gold-colored `G` as the symbol (via the `&6` color code).
- Formats balances with no decimal places, for example: `100 G` or `-50 G`.
- Overrides only the `fractional.places` setting; all other formatting follows the `default-number-formatter`.

---

## The `default-currency` setting

`default-currency` is a top-level configuration key that names the currency the legacy Vault adapter will use for balance operations. It must match the key of a currency defined in the `currencies` section.

This setting has no effect on VaultUnlocked. VaultUnlocked callers always specify a currency explicitly.

See [Legacy Vault Adapter](../legacy-vault-adapter/) for more detail.
