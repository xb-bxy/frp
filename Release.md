## Features

* Added `failAction` configuration option for server HTTP plugins. When a plugin is unreachable, behavior can now be configured per-plugin: `"reject"` (default) rejects the connection, `"allow"` allows the request to pass through. This is currently applied to the `NewUserConn` operation, enabling more resilient deployments where plugin downtime should not block user connections.

## Improvements

* Bumped server plugin API version from `0.1.0` to `0.2.0`, adding `FailAction()` to the `Plugin` interface.
* Added `failAction` field documentation and examples to `frps_full_example.toml`.

## Fixes

* Added missing `CloseUserConn` to the server plugin validation allowed operations list, so that configuring `ops = ["CloseUserConn"]` in `[[httpPlugins]]` no longer triggers a validation error.