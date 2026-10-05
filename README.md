# transport-asset

An HTTP edge that serves immutable, content-addressed asset chunks to callers whose access relations allow the read.

## What it is for

Large assets such as worlds and avatars are stored as chunks named by their hash. Nothing about a chunk changes between ticks, so the edge shares no state with a zone and can run on any machine, in any region, behind any cache. A request resolves the chunk by its hash, checks the caller against the access-control code generated from `lean-rebac-core` and vendored under `gen/`, and sends the bytes.

## Build and run

```sh
cmake -B build
cmake --build build
```

The repository holds the request handler only. It has no entry point, and its chunk-store and caller-lookup functions are declared without definitions, so the build does not produce a server.

## Licence

MIT; see LICENSE.
