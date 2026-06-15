---
layout: page
title: Number Formatting
parent: Configuration
nav_order: 2
permalink: /configuration/number-formatting/
---

# Number Formatting

JConomy formats balance amounts before displaying them through the economy API. Formatting controls grouping separators, decimal places, rounding behavior, and the decimal separator character.

---

## Default number formatter

The `default-number-formatter` section in `config.yml` defines formatting rules that apply to all currencies unless a currency overrides them.

```yaml
default-number-formatter:
  grouping:
    group-size: 3
    separator: ','
  fractional:
    round: true
    places: 2
    separator: '.'
```

A currency can override any subset of these settings. See [Currencies](../currencies/) for how per-currency overrides work.

---

## Grouping settings

Grouping controls how the digits before the decimal point are separated into chunks for readability.

### `grouping.group-size`

The number of digits per group. Set to `0` to disable grouping entirely.

**Default:** `3`

| `group-size` | Example output |
|--------------|----------------|
| `3`          | `1,234,567`    |
| `0`          | `1234567`      |

### `grouping.separator`

The string used to separate digit groups. Set to an empty string (`''`) to produce no visible separator while still applying grouping logic.

**Default:** `','`

| `separator` | Example output |
|-------------|----------------|
| `','`       | `1,234,567`    |
| `'.'`       | `1.234.567`    |
| `''`        | `1234567`      |

---

## Fractional settings

Fractional settings control the decimal portion of the formatted amount.

### `fractional.places`

The number of digits to display after the decimal separator. Set to `0` to display no decimal portion at all; the separator is also dropped when `places` is `0`.

**Default:** `2`

| `places` | Example output |
|----------|----------------|
| `2`      | `1,234.56`     |
| `0`      | `1,234`        |

### `fractional.round`

Whether to round the displayed amount to the configured number of decimal places. When `true`, the value is rounded to the nearest representable value. When `false`, the value is always truncated (rounded toward zero).

**Default:** `true`

| `round` | Input   | `places: 2` output |
|---------|---------|--------------------|
| `true`  | `1.235` | `1.24`             |
| `false` | `1.235` | `1.23`             |

### `fractional.separator`

The string used to separate the integer part from the fractional part.

**Default:** `'.'`

| `separator` | Example output |
|-------------|----------------|
| `'.'`       | `1,234.56`     |
| `','`       | `1.234,56`     |
