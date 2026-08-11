# fabric-asset-edge

Chunks, addressed by their content, served to whoever is allowed to ask.

An **edge plane** is a plane with networking, and this one really is a server: an h2o loop
with handlers, answering requests. That is the difference between it and
`fabric-authority-plane`, which had an h2o loop and no handlers, and was a plane all along.

## Why it is not in anybody's domain

A domain is a packing of planes that share a ring, and a ring forces co-location. This
shares nothing per tick. A chunk is immutable and addressed by its hash, so there is no
state to exchange with a zone, no interest to filter, and nothing that goes stale between
one tick and the next.

So it lands wherever it likes — its own machine, its own region, several of them behind a
cache — and no zone cares where. That is the same reasoning that puts FoundationDB outside
`fabric-store-domain`, read from the other end.

Immutability is doing all the work here. It is what lets the cache be `immutable` and the
authorization be a check rather than a lock.

## The seam that is not wired

`casync_fetch_chunk` is the one call onto `idtxcli` (`fabric-flow-adapters`), `fetch` and
`verify`. Link its library or shell out to it: the handler, the addressing and the
authorization do not care which, which is why it is one function and not a dependency.

## State

**Not built.** `src/asset_distribution.c` is the handler, the addressing and the rebac gate,
carried over from `gyreplane` unchanged.

It needs h2o, and `gen/rebac.h`, which is generated from `lean-rebac-core` in the tree that
generates it. Copying that here would put one decision in two places. CMake says what is
missing rather than offering a target that cannot link.
