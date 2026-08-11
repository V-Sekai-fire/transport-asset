# gen/

Generated, and vendored here so this repository builds on its own.

`rebac.{c,h}` comes from `lean-rebac-core`. It is codegen output, checked in for the same reason
`thirdparty/harness` is a subtree and `lean-capstone` vendors capstone: a repository that
cannot build without fetching something else is not a repository, it is a note.

Self contained beats incomplete. The cost is that a copy can drift from the Lean that
produced it, and the answer to that is to regenerate rather than to edit these by hand —
which is also why they say `Do not edit` at the top.

`gyreplane` vendors the same files the same way, in `src/gen/`.
