---
layout: page
title: Experimental Features
nav_order: 6
permalink: /experimental/
has_children: true
---

# Experimental Features

{: .warning }
Experimental features are not stable. They may be incomplete, contain defects, or cause data loss or other undesirable behavior. They are not officially supported. They may change incompatibly or be removed entirely in any future version without notice.

JConomy exposes certain experimental features that are disabled by default. These features are made available for testing and early feedback, but they should not be used in production environments without careful evaluation.

---

## Enabling experimental features

Experimental features are controlled by a `features` section in `plugins/JConomy/config.yml`. This section is not present in the default configuration file and must be added manually.

The general structure is:

```yaml
features:
  feature-name:
    enabled: true
```

Each feature has its own key under `features`. Only features you explicitly enable are activated.

---

## Available experimental features

| Feature | Key | Description |
|---|---|---|
| [Data Transfer](data-transfer/) | `data-transfer` | Import and export player account data between economy providers |
