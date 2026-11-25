# Changelog

Every release of apogee-pennant, newest first. Each is a `vX.Y.Z` tag with a GitHub
release carrying its section; the `grav` line in `grav.toml` says which
compiler versions it builds with.

## Unreleased

Needs grav 1.12.0 or newer.

- `PennantPackage`: the module connects itself to an apogee-framework app (apogee-package):
  its settings, wiring moved here from the framework.
- Its tables as a migration (`CreateFeaturesTable`, run by `migrate`); the store's own
  table-making method uses the same definition.

## 0.1.1 (2026-11-21)

- Values read or resolved are kept in the process for `cacheMs` (1 s by
  default): a repeated check asks the store nothing (about 50 times faster on
  SQLite, more over a network). Changes made through Pennant show at once;
  `Feature.flushCache()`.

## 0.1.0 (2026-11-21)

The first release. Needs grav 1.10.1 or newer.

- `Feature` and `Features`: resolvers decided once per scope and stored, rich
  values, activating and deactivating for a scope or everyone, forgetting and
  purging.
- `Rollout.percent` and `Rollout.pick`: stable percentage rollouts and variants.
- Memory, database (grav-db) and cache (grav-cache) stores.
- `EnsureFeature` route middleware.
