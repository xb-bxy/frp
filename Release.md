## Features

* Added a built-in `store` capability for frpc, including persisted store source (`[store] path = "..."`), Store CRUD admin APIs (`/api/store/proxies*`, `/api/store/visitors*`) with runtime reload, and Store management pages in the frpc web dashboard.
* Server plugin system now supports a `CloseUserConn` operation. When a user TCP connection is closed, frps notifies all registered plugins with user info, proxy name, proxy type, and remote address, enabling external systems to track full connection lifecycle events such as audit logging and session cleanup.
## Improvements

* Kept proxy/visitor names as raw config names during completion; moved user-prefix handling to explicit wire-level naming logic.
* Added `noweb` build tag to allow compiling without frontend assets. `make build` now auto-detects missing `web/*/dist` directories and skips embedding, so a fresh clone can build without running `make web` first. The dashboard gracefully returns 404 when assets are not embedded.
* Improved config parsing errors: for `.toml` files, syntax errors now return immediately with parser position details (line/column when available) instead of falling through to YAML/JSON parsing, and TOML type mismatches report field-level errors without misleading line numbers.
* Removed duplicated `startVisitorListener` and `buildDomains` helper methods from `BaseProxy`, consolidated into shared abstractions.
* Capture `userRemoteAddr` before `libio.Join` to avoid reading from a potentially closed connection in debug logs.

## Fixes

* Fix error variable shadowing in `GetWorkConnFromPool`: inner `err :=` declaration prevented the outer `err` from being set, which could cause the function to return a closed connection with a nil error.