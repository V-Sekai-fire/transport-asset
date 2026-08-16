# transport-asset

Chunks, addressed by their content, served to whoever is allowed to ask.

A **transport layer** is the input that triggers an interactor, and this one really is a
server: an h2o loop with handlers, answering requests. That is the difference between it and
`interactor-authority`, which had an h2o loop and no handlers, and was an interactor all along.

## Why it shares nobody's ring

A ring forces co-location on the interactors packed into it. This shares nothing per tick. A
chunk is immutable and addressed by its hash, so there is no state to exchange with a zone, no
interest to filter, and nothing that goes stale between one tick and the next.

So it lands wherever it likes — its own machine, its own region, several of them behind a
cache — and no zone cares where. That is the same reasoning that puts FoundationDB outside
`datasource-queen`, read from the other end.

Immutability is doing all the work here. It is what lets the cache be `immutable` and the
authorization be a check rather than a lock.

## The seam that is not wired

`casync_fetch_chunk` is the one call onto `idtxcli` (`datasource-flow`), `fetch` and
`verify`. Link its library or shell out to it: the handler, the addressing and the
authorization do not care which, which is why it is one function and not a dependency.

## State

`src/asset_distribution.c` is the handler, the addressing and the rebac gate, carried over
from `gyreplane` unchanged. It builds, and CI builds it.

`gen/rebac.{c,h}` is vendored here rather than fetched. A copy can drift from the Lean that
produced it, and that is the smaller problem: a repository that cannot build without cloning
another one is a note, not a repository. Regenerate those files, never edit them.

h2o is a system library and is required outright, the way `datasource-store` requires
libfdb_c and libsqlite3.
