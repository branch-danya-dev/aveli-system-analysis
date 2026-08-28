# Drift

> Typed Flutter data-access technology над device-local SQLite workspace.

`drift 2.34.3`

```text
Flutter repositories
      ↓
Drift
      ↓
SQLite
```

Canonical physical model: [`../../../database/local/`](../../../database/local/)

**Replaceability: Medium.** Replacement при сохранении SQLite затронет repositories, generated models, reactive reads, migrations и database tests.
