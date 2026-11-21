# Changelog

Every release of apogee-pennant, newest first. Each is a `vX.Y.Z` tag with a GitHub
release carrying its section; the `grav` line in `grav.toml` says which
compiler versions it builds with.

## Unreleased

## 0.1.0 (2026-11-21)

The first release. Needs grav 1.10.1 or newer.

- `Feature` and `Features`: resolvers decided once per scope and stored, rich
  values, activating and deactivating for a scope or everyone, forgetting and
  purging.
- `Rollout.percent` and `Rollout.pick`: stable percentage rollouts and variants.
- Memory, database (grav-db) and cache (grav-cache) stores.
- `EnsureFeature` route middleware.
