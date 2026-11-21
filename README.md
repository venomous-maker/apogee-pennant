# apogee-pennant

Feature flags for [Apogee](https://github.com/venomous-maker) apps: whether a
feature is on, for whom, and with which value. A feature is a resolver that
decides for a scope (a user, a team, everyone) the first time it is asked; the
answer is stored, so it stays the same until it is changed on purpose.

```grav
import "std"
import "apogee-pennant"
use apogee.*

Feature.define("new-checkout", fn(scope: any) -> any { return (scope as User).isBeta })
Feature.define("button-color", fn(scope: any) -> any { return Rollout.pick(scope, "button-color", ["blue", "green"]) })
Feature.define("search-v2", fn(scope: any) -> any { return Rollout.percent(scope, "search-v2", 25) })

if (Feature.for(user).active("new-checkout")) { ... }
let color = Feature.for(user).value("button-color") as string
Feature.activateForEveryone("search-v2")
router.aliasMiddleware("feature", fn(a: Array<string>) -> Middleware { return new EnsureFeature(Str.join(a, ",")) })
router.get("/checkout", ...).middleware(["feature:new-checkout"])
```

## Checking

`Feature.for(scope)` gives a scope's flags: `active(f)`, `inactive(f)`,
`value(f)` (true/false, or a value such as a variant), `values([...])`,
`allAreActive([...])`, `someAreActive([...])`, `allAreInactive([...])`,
`when(f, onActive, onInactive)`. `Feature.active(f)` and `Feature.value(f)`
are for everyone (the null scope). A feature with no definition is off.

A scope is a string, a number, a `FeatureScope` (its `featureScopeId()`), or an
object with an `id` field that reflection reads (Orbit models; another class
needs `reflect(...)`); anything else is an error, so two objects never share
flags by accident.

## Changing

| | |
|---|---|
| `Feature.for(s).activate(f, value = true)`, `.deactivate(f)` | for one scope |
| `Feature.for(s).forget(f)` | the resolver decides again next time |
| `Feature.activateForEveryone(f, value)`, `deactivateForEveryone(f)` | every scope stored so far, and everyone |
| `Feature.purge(f)`, `Feature.purge()` | forget a feature's stored values, or all |

## Rollouts

`Rollout.percent(scope, feature, percent)` and `Rollout.pick(scope, feature, variants)`
hash the feature and the scope, so a scope always lands on the same side even
before anything is stored, and each feature splits scopes differently.

## Stores

| | |
|---|---|
| `MemoryFeatureStore` | one process (the default) |
| `DatabaseFeatureStore(pool, table = "features")` | SQLite, MySQL or PostgreSQL through grav-db; `createTable()` |
| `CacheFeatureStore(cacheStore, prefix = "features:")` | any grav-cache store: Redis, files, a database, memory |

`Feature.useStore(store)` picks one; `Features` is the manager behind the facade.

## Routes

`new EnsureFeature("a,b", status = 404)` (registered under a middleware alias,
as above, so routes name it: `"feature:new-checkout"`) lets a request through only when the
request's user (or everyone, without one) has every listed feature on;
otherwise 404, as if the route were not there (or the status given).

## Tests

```bash
grav test tests/pennant.grav
```
