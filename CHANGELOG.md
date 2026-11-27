# Changelog

Every release of apogee-pennant, newest first. Each is a `vX.Y.Z` tag with a GitHub
release carrying its section; the `grav` line in `grav.toml` says which
compiler versions it builds with.

## Unreleased

## 0.2.1 (2026-11-27)

### Fixes

- Database connections that the server dropped while idle (MySQL's `wait_timeout`, a restart, a proxy's idle limit) reconnect on their next use instead of failing with "server has gone away" (grav-db 0.2.1). Uses the new releases of apogee-package, grav-cache, grav-db, grav-orbit.

## 0.2.0 (2026-11-25)

Needs grav 1.12.0 or newer.

- Values kept a moment in a sharded, bounded LruCache (no lock shared by every
  check); needs grav 1.13.0 or newer.
- `PennantPackage`: the module connects itself to an apogee-framework app (apogee-package):
  its settings, wiring moved here from the framework.
- Its tables as a migration (`CreateFeaturesTable`, run by `migrate`); the store's own
  table-making method uses the same definition.

### Dependencies

- Uses apogee-package 0.1.0, apogee-routing 0.3.0, grav-orbit 0.3.0, apogee-http 0.3.0, grav-cache 0.2.2 (from their GitHub tags).

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
